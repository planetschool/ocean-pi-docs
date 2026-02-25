---
sidebar_position: 4
---

# Adding Color Animation

Currently, our code only polls the color sensor once, then updates `f`, then we update `whale`. But we want to do this continuously while our program is running to give our creature life. Since *Wonder* will very likely be moving while your code is running, the light conditions will change from one moment to the next. We will need to use a loop to accomplish this. A loop causes a portion of your code to run over and over again. Specifically, we will use something called a `while` loop. 

:::tip[Pro tip]

To indent a large portion of your code (like all the code you want to be inside your `while` loop), highlight everything you want to indent and hit Tab. If you want to unindent your code, hold Shift and press Tab.

:::

Let's modify our `# Using the color sensor` section to include a `while` loop:

```python
# -------------------------------
# Using the color sensor
# -------------------------------

## Set up the sensor
my_sense_hat.color.gain = 60 # Set the sensitivity of the sensor
my_sense_hat.color.integration_cycles = 64 # The interval at which the reading will be taken

while True:
    # Use the sensor
    rgb = my_sense_hat.color # get the colour from the sensor
    f = (rgb.red, rgb.green, rgb.blue) # re-assign the variable "f" to be the sensed color

    # Arrange my colors on an 8x8 matrix.
    image = [
      f, f, f, g, g, g, g, g,
      f, f, g, g, f, g, g, f,
      f, f, f, f, f, f, g, g,
      f, g, g, g, g, g, g, g,
      g, g, g, g, g, g, d, d,
      g, b, g, g, g, d, d, f,
      g, g, g, g, d, d, f, f,
      f, g, g, d, d, f, f, f]

    # Code telling the pixels to light up
    my_sense_hat.set_pixels(image)
```

Very literally, this loop will keep running *while* a condition is true. So, we could write `while f == (25, 25, 112)` and our loop would end once we change `f` to the value derived from the color sensor. Notice also that we did not include the code where we calibrate the sensor in our loop. We only need to calibrate our sensor once, we don't need to recalibrate it constantly.

For now, we want this loop to run “forever” or, more realistically, until we choose to terminate the loop by clicking “stop” on our program. Therefore, we could write something like `while 1 == 1`, `while 1 + 1 == 2`, or `while 2 > 1` since all of these mathematical statements will always be true. Usually, though, we just skip coming up with a true expression and just write while `True`.

:::danger[Look out]

Pay attention to the difference between `=` and `==`. A single equals sign is used to declare variables. It is a command: "thou shalt have this value." A double equals sign means "equivalent to" and is more akin to how we use an equals sign in math. Mixing these up can be a common source of your program returning an error.

:::



If you run this code in Trinket, then you will need to select a new color from the “Colour” selection panel, which is how you manipulate the light that the emulated sensor is sensing. Every time you change this color, the background color of the whale should update to this new color.

Here is the full program at this point:

```python
## Libraries
from sense_hat import SenseHAT
my_sense_hat = SenseHAT()
from time import sleep

# -------------------------------
# Creature design
# -------------------------------

## Declare my colors as RGB values and store them in variables
d = (255, 255, 255) # Cyan
f = (25, 25, 112) # MidnightBlue
g = (0, 191, 255) # DeepSkyBlue
b = (0, 0, 0) # Black

## Arrange my colors on an 8x8 matrix to make an image. This is a whale I made.
whale = [
  f, f, f, g, g, g, g, g,
  f, f, g, g, f, g, g, f,
  f, f, f, f, f, f, g, g,
  f, g, g, g, g, g, g, g,
  g, g, g, g, g, g, d, d,
  g, b, g, g, g, d, d, f,
  g, g, g, g, d, d, f, f,
  f, g, g, d, d, f, f, f]

## Code telling the pixels to light up
my_sense_hat.set_pixels(whale)

## Pause the program to show this happy, beautiful whale
sleep(1) # numbers are in seconds

# -------------------------------
# Using the color sensor
# -------------------------------

## Set up the sensor
my_sense_hat.color.gain = 60 # Set the sensitivity of the sensor
my_sense_hat.color.integration_cycles = 64 # The interval at which the reading will be taken

while True:
    ## Use the sensor
    rgb = my_sense_hat.color # get the color from the sensor
    f = (rgb.red, rgb.green, rgb.blue) # re-assign the variable "f" to be the sensed color

    ## Re-declare the contents of "whale" now that "f" is storing a new value.
    whale = [
    f, f, f, g, g, g, g, g,
    f, f, g, g, f, g, g, f,
    f, f, f, f, f, f, g, g,
    f, g, g, g, g, g, g, g,
    g, g, g, g, g, g, d, d,
    g, b, g, g, g, d, d, f,
    g, g, g, g, d, d, f, f,
    f, g, g, d, d, f, f, f]


    ## Code telling the pixels to light up, but with a new "f" value
    my_sense_hat.set_pixels(whale)
```