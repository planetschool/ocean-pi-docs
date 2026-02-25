---
sidebar_position: 7
---

# Adding a Camera

Notes from the addition of the Raspberry Pi AI Camera to the Ocean Buoy. The documentation I am following for adding the camera can be found here: https://www.raspberrypi.com/documentation/accessories/ai-camera.html

Beginning by installing the required software:

```bash
sudo apt install imx500-all
```

After rebooting, running the following command gives a live demo of the camera and its inference capabilities:

```bash
rpicam-hello -t 0s --post-process-file /usr/share/rpi-camera-assets/imx500_mobilenet_ssd.json --viewfinder-width 1920 --viewfinder-height 1080 --framerate 30
```

Very cool! 

The big question is whether it already has the default capacity to recognize fish in an underwater setting or whether we need to create that inference capability.

## Sending Data via MQTT

We need to encode our image as a string of characters in order to send it. We also need to control the quality and resolution. When I tried to send an image without adjusting these parameters, the image was 40 million characters long and Thingsboard refused to accept it. With the following code, we will capture an image with specific size parameters, change its quality, and turn it into a string of characters. To do this, we will need the following libraries:

```python
import base64
from picamera2 import Picamera2
from picamera2.encoders import JpegEncoder
from PIL import Image
import io
```

Then we will create a new section to get our camera configured and running prior to the loop:

```python
#----------------------------------------------------------------------
# --- Camera Setup --- #
#----------------------------------------------------------------------
Camera_On = True

picam2 = Picamera2()
config = picam2.create_still_configuration(main={"size": (640, 480)})
picam2.configure(config)
picam2.start()

def create_snapshot():
	# Capture raw RGB frame
	frame = picam2.capture_array()

	# Encode to JPEG with given quality
	buf = io.BytesIO()
	Image.fromarray(frame).save(buf, format="JPEG", quality=50)  #control size here
	jpeg_bytes = buf.getvalue()
	return jpeg_bytes
```

Then we need to add some code to inside our `while` loop to capture repeated images. Add the following:

```python
	if Camera_On:
		jpeg_bytes = create_snapshot()
		pic = base64.b64encode(jpeg_bytes).decode("utf-8")
		print("length of the pic is:", len(pic))
		payload["snapshot"] = pic
```

Since I was trying to figure out the size of my image and how small I needed to get it before Thingsboard would open the gates, I included the `print` statement `print("length of the pic is:", len(pic))` which will output the string length to your terminal. If you are not interested in this, you can comment our or delete this line of code. With the settings in the code above, I got the image down to between 16000-17000 characters—much better than 40 million!

If your JSON dictionary is not named `payload` then you will get an error, so make sure you are adding `"snapshot"` to the proper dictionary.

## Thingsboard Integration

To get an image to show up on Thingsboard, you need to create a custom HTML widget that includes the following code in “Appearance.” You can delete all of the HTML code that you find in there and replace it with this:

```html
<div class='card'>
  <div class='content' style="text-align:center;">
    <img style="max-width: 100%; height: auto; margin-top: 10px;"
         src="data:image/jpeg;base64,${snapshot}">
  </div>
</div>
```