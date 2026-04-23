# Adding An Analog Sensor

Unless you are using higher end, more expensive lab-grade sensors like what we have installed in our aquaculture lab and on *Wonder*, the water conductivity sensor, water temperature sensor, and turbidity sensor you are using are analog sensors. The Raspberry Pi does not have an analog GPIO pin, so we need to use a converter. 

Included in the Ocean Pi Kit is a small breadboard that you can use to connect the Analog Digital Converter (ADC). For the BME280 sensor, you connected it directly to the GPIO pins of the Raspberry Pi. Since we need to attach multiple sensors to the SDA and SCL GPIO pins, we need to use the breadboard to split and share those pins. So unplug your BME280 sensor and attach the ADC via the breadboard in the same way you attached the BME280 sensor. 

A little background on breadboards. A breadboard is a series of "rails" or bus bars that join wires into a common circuit. On these small breadboards, the horizontal rows with 5 holes are all connected. So if you send 3.3 volts from the Raspberry Pi to one of those holes, the other 4 holes will also have 3.3 volts. In the photo below, I have electrified the top left row with 3.3 volts.

![Breadboard example1](./img/breadboard_example1.jpeg)

Larger breadboards have dedicated rails for power that are labeled as such, but we can use any rail we like for power. The central divider is a break between the left and right side of the board. As an example, in the below photo, the red wire is sending 3.3 volts from the Raspberry Pi. The green wire receives no power because it is on the other side of the central divider. The blue wire also receives no power because it is on a different row. Only the white wire receives 3.3 volts.

![Breadboard example2](./img/breadboard_example.jpeg)

Before you start adding wires, press the ADC into the board and then bring wires to the board and into the appropriate slots. VDD = 3.3 volts, GND = ground, SDA = GPIO 2, and SCL = GPIO 3. 

![ADC example](./img/ADC_example.jpeg)