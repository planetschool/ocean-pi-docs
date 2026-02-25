---
sidebar_position: 5
---

# Displaying Data

Download a library for communicating with the LCD screen, which I got here: https://gist.github.com/DenisFromHR/cc863375a6e19dce359d.

To install it I ran the command:

```python
wget https://gist.githubusercontent.com/DenisFromHR/cc863375a6e19dce359d/raw/36b82e787450d127f5019a40e0a55b08bd43435a/RPi_I2C_driver.py
```

Then we can add the following section to our code:

```jsx
#### LCD Display
LCD_On = True                                                                                                    
if LCD_On:
	import I2C_LCD_driver
	
	mylcd = I2C_LCD_driver.lcd()
	mylcd.lcd_display_string("Starting up the", 1, 0)
	mylcd.lcd_display_string("Weather Station", 2, 0)
```

The `LCD_On` variable allows us to turn the LCD part of the code on and off by changing this value to True or False.

Next, have a look at the code I wrote for my sensor array and see how you might need to modify parts of it for your needs in creating a LCD display for your sensors.

```python
if LCD_On:
	mylcd.lcd_clear()
	if Light_Sensor_On:
		mylcd.lcd_clear()
		mylcd.lcd_display_string("Bright: {} Lux".format(lux), 1, 0)
		mylcd.lcd_display_string("IR: {} , Vis: {}".format(IR, visible), 2, 0)
		sleep(2)
	if BME280_Sensor_On:
		mylcd.lcd_clear()
		mylcd.lcd_display_string("Press: {} hPa".format(BME_pressure), 1, 0)
		mylcd.lcd_display_string("Humid: {} %".format(BME_humidity), 2, 0)
		sleep(2)
	if CO2_Sensor_On:
		mylcd.lcd_clear()
		mylcd.lcd_display_string("Temp: {} *F".format(CO2_temp_F), 1, 0)
		mylcd.lcd_display_string("CO2: {} ppm".format(CO2_CO2), 2, 0) 
		sleep(2)
		mylcd.lcd_clear()
		mylcd.lcd_display_string("Humid: {} %".format(CO2_humidity), 1, 0)
		sleep(2)
	if Color_Sensor_On:
		mylcd.lcd_clear()
		mylcd.lcd_display_string("R:" + str(red) + " G:" + str(green) + " B:" + str(blue), 1, 0)
		sleep(2)
```