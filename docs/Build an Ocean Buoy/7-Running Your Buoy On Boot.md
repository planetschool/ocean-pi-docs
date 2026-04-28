---
sidebar_position: 7
---

# 7. Running Your Buoy On Boot

One of the final steps in making a functional buoy (or remote sensor array of any kind) is enabling your Raspberry Pi to start running your code automatically when it turns on. Otherwise, you would need to bring a monitor, keyboard, and mouse out to wherever you are deploying your Raspberry Pi, turn it on, activate your virtual environment, and run your program. More often than not, that is not a practical or desirable thing to do. 

In order to have our Raspberry Pi run our program when it boots up, we will create a service that will essentially type commands into the terminal for us. We also need to modify some elements of how our code sends data to Thingsboard. We want to make sure that, if the connection takes a minute to get established, the code will keep trying to connect instead of terminating with an error. We also don't want to keep looping and gathering data from our sensors if the broadcast connection isn't there. Let's start with the service. We will create a file called `oceanpi.service`

Run `sudo nano /etc/systemd/system/oceanpi.service` in terminal. This will create a new empty file that you should paste the following into:

```bash
[Unit]
Description=Run code at boot
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=planetschool
WorkingDirectory=/home/planetschool/ocean-pi
ExecStartPre=/bin/sleep 15
ExecStart=/home/planetschool/venv-ocean-pi/bin/python3 /home/planetschool/ocean-pi/Ocean-Pi-Buoy-Project.py>
Restart=on-failure
RestartSec=10
Environment=PYTHONUNBUFFERED=1
Environment=THINGSBOARD_TOKEN=your_token_here

[Install]
WantedBy=multi-user.target
```

There are several items in the code above that you need to change for your personal Raspberry Pi and your project:
* Your Raspberry Pi username needs to go in `User`
* I store all my Ocean Pi files in a folder called "ocean-pi". Whatever folder (directory) you have your project files in needs to go in `WorkingDirectory`
* The name of your virtual environment needs to go into `ExecStart` where I have `venv-ocean-pi`, which is the name of my virtual environment
* The filepath of your code needs to replace `/home/planetschool/ocean-pi/Ocean-Pi-Buoy-Project.py`
* `THINGSBOARD_TOKEN` needs to have your secret token code

Once all of that is updated, follow the same procedure from when you last used Nano to edit the .bashrc file. Hit Ctrl+X, type "Y" to save, then hit "Enter/Return" to keep the filename "oceanpi.service".

We have just created a service that will run when the Raspberry Pi boots up. Now we need to activate it. Run the following commands in terminal:

```bash
sudo systemctl daemon-reload
sudo systemctl enable oceanpi.service
sudo systemctl start oceanpi-service
journalctl -u oceanpi.service -f --no-pager
```

If you navigate to your device in Thingsboard and click on "Latest Telemetry", these commands should have started your program and begun broadcasting data. If not, don't worry. It might be because your data connection wasn't established. Let's modify our broadcasting code to make it more resilient.

Right now, your program looks something like this:

```python
## Dashboard
THINGSBOARD_TOKEN = os.environ.get("THINGSBOARD_TOKEN")
THINGSBOARD_HOST = "thingsboard.cloud"
PORT = 1883
MQTT_TOPIC = "v1/devices/me/telemetry"
client = mqtt.Client()
client.username_pw_set(THINGSBOARD_TOKEN)
client.connect(THINGSBOARD_HOST, PORT, 60)
client.loop_start()
```

Change that to this:

```python
## Dashboard
THINGSBOARD_TOKEN = os.environ.get("THINGSBOARD_TOKEN")
THINGSBOARD_HOST = "thingsboard.cloud"
PORT = 1883
PUBLISH_INTERVAL = 10  # seconds
MQTT_TOPIC = "v1/devices/me/telemetry"

print(f"[INFO] THINGSBOARD_TOKEN present: {THINGSBOARD_TOKEN is not None}", flush=True)
mqtt_connected = False

def on_connect(client, userdata, flags, rc, properties=None):
    global mqtt_connected
    print(f"[INFO] MQTT on_connect rc={rc}", flush=True)
    mqtt_connected = (rc == 0)

def on_disconnect(client, userdata, rc, properties=None):
    global mqtt_connected
    mqtt_connected = False
    print(f"[WARN] MQTT disconnected rc={rc}", flush=True)

def start_mqtt():
    global client

    if not THINGSBOARD_TOKEN:
        raise RuntimeError("THINGSBOARD_TOKEN is not set")

    client = mqtt.Client()
    client.username_pw_set(THINGSBOARD_TOKEN)
    client.on_connect = on_connect
    client.on_disconnect = on_disconnect

    print(f"[INFO] Connecting to {THINGSBOARD_HOST}:{PORT} ...", flush=True)
    client.connect(THINGSBOARD_HOST, PORT, 60)
    client.loop_start()

    for _ in range(30):
        if mqtt_connected:
            print("[INFO] MQTT connected successfully", flush=True)
            return client
        print("[INFO] Waiting for MQTT connection...", flush=True)
        time.sleep(1)

    raise RuntimeError("MQTT did not connect within 30 seconds")

client = start_mqtt()
```

Similarly, in our `while` loop, we want to modify the commands to send the data and, if there is an error in sending it, try to re-establish a connection. Your code currently looks something like this:

```python
## The Code!
data = {}
while True:
    import board
    from adafruit_bme280 import basic as adafruit_bme280
    i2c_port = 1  #This is a Raspberry Pi setting
    bme280_address = 0x77  #The address of our sensor
    i2c = board.I2C()  #Accessing the I2C library
    bme280 = adafruit_bme280.Adafruit_BME280_I2C(i2c, int(bme280_address)) #Accessing the BME280 library and connect to sensor via I2C

    data["humidity"] = bme280.humidity #Get humidity from the sensor
    data["turbidity_value"] = turbidity_sensor.value #Get value from the analog sensor
    data["turbidity_volts"] = turbidity_sensor.voltage #Get voltage from the analog sensor
    client.publish(MQTT_TOPIC, json.dumps(data))
```

Modify that to look like this:

```python
## The Code!
data = {}
while True:
    ## Data Gathering
    data["humidity"] = bme280.humidity #Get humidity from the sensor
    data["turbidity_value"] = turbidity_sensor.value #Get value from the analog sensor
    data["turbidity_volts"] = turbidity_sensor.voltage #Get voltage from the analog sensor

    ## Data Broadasting
	if mqtt_connected:
		print("Payload about to send:", payload, flush=True)
		result = client.publish(MQTT_TOPIC, json.dumps(payload))
		result.wait_for_publish()
		print(f"[INFO] Publish rc={result.rc}", flush=True)
	else:
		print("[ERROR] MQTT not connected, skipping publish", flush=True)
		
	if not mqtt_connected:
		print("[WARN] MQTT not connected, attempting reconnect...", flush=True)
		try:
			client.reconnect()
			time.sleep(2)
		except Exception as e:
			print(f"[ERROR] Reconnect failed: {e}", flush=True)
			time.sleep(5)
			continue
```

Notice that I added two comment lines to break up and label what is happening inside the loop.