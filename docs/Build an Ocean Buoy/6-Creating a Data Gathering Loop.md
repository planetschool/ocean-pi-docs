---
sidebar_position: 6
---

# 6. Creating a Data Gathering Loop

Nice work, you just got a piece of data from your analog sensor! You now have data coming digitally via I2C from the BME280 sensor and analog data coming from the turbidity sensor in the form of voltage. That analog data is sent to an analog to digital converter (ADC) and then it too is sent via I2C. This is huge progress!

The code we wrote asks our sensors for the data and then stores that data in variables. It does this process once and then the program finishes. We want the code to keep doing it—we want to continuously monitor water conditions—so we will use something called a loop. Specifically we will use a loop that continues to run the same code over and over again while a condition remains true. Fittingly, it is called a `while` loop.

We can create a `while` loop that will only run for a certain number of loops and the code in the loop eventually causes the loop to terminate. Look at the following code:

```python
counter = 0
while counter < 5:
    print("The loop is looping!")
    print("The counter is ", counter)
    counter += 1
```
Read this code and think about what it might do.

It will create the following output:

```bash
The loop is looping!
The counter is 0
The loop is looping!
The counter is 1
The loop is looping!
The counter is 2
The loop is looping!
The counter is 3
The loop is looping!
The counter is 4
```

If you guessed the loop would run five times you were right. But notice that the loop terminates on 4 since 5 is not less than 5, it is equal. For our loop, we want it to run continuously while the buoy has power. Therefore, we need something that is true constantly. We will use `while True`, but you can use any true statement. For more on `while` loops, check out our [Send An Image To Sea](/docs/Send An Image To Sea) project.

We need to put our code that polls the sensors into the `while` loop.

```python
## The Code!
data = {}
while True:
    data["humidity"] = bme280.humidity #Get humidity from the sensor
    data["turbidity_value"] = turbidity_sensor.value #Get value from the analog sensor
    data["turbidity_volts"] = turbidity_sensor.voltage #Get voltage from the analog sensor
    client.publish(MQTT_TOPIC, json.dumps(data))
```

Notice how I removed the `humidity = bme280.humidity` line and just consolidated the role that the variable `humidity` was playing. You can choose to keep that step if you prefer. What is essential is that you put data gathering commands and the data broadcasting commands into the loop. You do not want to put the sensor initialization in the loop as that will just slow down your program. Think of it this way: you want the things that will be different over time to be in the loop because that is what our code is actually monitoring. So the following code would be wrong:

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

This program will run. It won't give you an error. But it is wasteful. `bme280` will always be equal to `adafruit_bme280.Adafruit_BME280_I2C(i2c, int(bme280_address))`, so why re-run that command on every loop? `bme280_address` should always be `0x77`, why say it over and over again? Better to say those things once at the start of your program and then have your loop re-ask "What is the humiditiy?" over and over again.