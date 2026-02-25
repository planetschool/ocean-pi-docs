---
sidebar_position: 3
---

# Using the Color Sensor

Now we will add code for using the Sense HAT’s color sensor. 

Just like your eyes adjust for how bright the surroundings are, we need to tell our color sensor how sensitive it needs to be. Gain is kinda like telling the color sensor whether it needs to squint or open its eyes as wide as possible. There are four possible values for gain: 1, 4, 16, and 60. If you have a Sense HAT and Raspberry Pi, you can do experiments in different lighting scenarios to see the different results you get from different gain settings. We recommend setting gain to either 16 or 60.

:::tip[Pro tip]

Keep it up with the comments as we continue the project. Typing `#` tells the computer to ignore whatever is written after the `#`. We call this a comment. Comments can take up the entire line or come after actual code (see below). You can use multiple `#` to denote hierarchy: one `#` is a major section and `##` is a subsection, for example. Since the first `#` is all that matters for creating a comment, you can write anything after the first `#` that you want (including another `#`). Use whatever commenting convention works for you!

:::
 
## Tuning the sensor

The following code should be added to your existing project after what you have already written:

```python
# -------------------------------
# Using the color sensor
# -------------------------------

## Set up the sensor
my_sense_hat.color.gain = 60 #Set the sensitivity of the sensor
my_sense_hat.color.integration_cycles = 64 #Interval at which readings will be taken
```

## Change the background color

The first thing we will do is grab a color from the color sensor and make that the background color. You can choose to have the color sensor affect any of the colors in your design. You could even have your color sensor only affect one portion of the color, like just the amount of green. But let's start with the background color as the example and then you can get as crazy as you like.

We will also need to pause our program in between our original image and our sensor-based image. This requires that we import another library called `sleep` at the start of the program. If we don’t use `sleep()` then our first image will be displayed so quickly that we won’t see it, since the code will be read and run as quickly as the computer’s processor can process it.

I also decided to change the name of my image from `image`, which is boring, to `whale`, which is more descriptive. I just need to make sure all of my references to `image` get updated.


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

## Rebuilding your image

Notice how, at first, `f` is assigned the value of Midnight Blue, which has an RGB value of (25, 25, 112). Then, later in the code, we change `f` to be whatever RGB values the color sensor collects. This is the beauty of variables—we can change their value at any time. What is important to notice here, though, is that if we did not restate `whale` after redefining `f`, then there would be no change. You can see this if you delete the second `whale` array and run the code: your background will not change color. Why does this happen?

When you first define:
```python
whale = [
  f, f, f, g, g, g, ...
]
```
Python does not store the variable name `f` inside the list.

It stores the current value that `f` refers to at that moment.

So originally:

```python
f = (25, 25, 112)
```

The list `whale` actually becomes:
```python
[(25,25,112), (25,25,112), ...]
```

This is because variables like `f` hold references to objects and lists store objects, not variable names. So later, when you do:

```python
f = (rgb.red, rgb.green, rgb.blue)
```
You are changing what `f` points to, but the list still contains the old values since it never actually stored `f` in the list. Therefore, redefining `f` does not retroactively update the list.

That’s why you must rebuild `whale`.

## Changing more colors

When you run 

```python
rgb = my_sense_hat.color 
```
you are getting a color value. You can see what this is by adding a `print()` statement. 

:::tip[Pro tip]

`print()` statements are extremely useful when you are writing code. They can help reveal what something is, tell you how far your program is executing before hitting an error, and whether `if` statements or loops are actually triggering. Don't be afraid to put a `print()` statement anywhere you are wondering what is happening.

:::

```python
rgb = my_sense_hat.color 
print(rgb)
print(rgb.red, rgb.blue, rgb.green)
```

When you run your code with these lines added (you can add them anywhere after `rgb = my_sense_hat.color`), you should see output to your terminal. Here is what I get in terminal when I run this:
```python
<sense_hat.hat.ColourSensor object>
(216, 19, 19)
```
So `rgb` is something called a `<sense_hat.hat.ColourSensor object>` and inside that object are three values named red, green, and blue. If you want to play with other colors in your image, you can use some or all of these values. I am going to store the blue value in a new variable:

```python
bluey_blue = rgb.blue
print(bluey_blue)
```
Sure enough, the terminal prints `19`, confirming that I have stored whatever amount of blue the color sensor is seeing in my variable `bluey_blue`. So now, if I want to make my whale as blue as whatever the color sensor is seeing, I can change the `g` variable, which is where I had stored my whale's skin color. But I don't have to change the whole color:

```python
g = (0, 191, bluey_blue) # DeepSkyBlue...maybe
```

I kept the red and green values of my original `g` color, but how blue it is will change. Don't forget to redeclare your `image` (in my case, `whale`) after you change colors of a variable in the image. 

But who says I have to use the blue value in the blue slot?

```python
g = (0, bluey_blue, 255) # DeepSkyBlue...maybe
```

Here I am taking the value of blue that the color sensor sees and using it to change how green my whale's skin is. If my Sense HAT were in a very blue room, then my whale would turn very green.

Time to experiment with your color sensor and how it affects your image!