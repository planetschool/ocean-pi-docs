---
slug: Latest Progress Update
title: Latest WX station Progress Update
authors: aram
---

Since my last update, progress has slowed. Two blizzards, a cold, and a school break were the main reasons. 
We resolved our camera problems by creating a virtual environment for the software on the Raspberry Pi. 
We also updated and reinstalled the Adafruit-Blinka libraries. 
Additionally, we removed the code that automatically started the program when the Raspberry Pi powered on. 
Now, it no longer tries to access and occupy the camera at startup. All that is left to sort through in the 
WX station software is figuring out why the information from the wind vane and anemometer that prints in the 
terminal and on the LCD screen only shows a few characters from the written code, for example, it will print 
something like “Char is b. (a,0..) (..1,c) (..0, a)”, instead of collected data for wind speed and direction 
from the atmosphere. Now that a 3D model for a Stevenson screen has been selected, I will be printing a first 
rendition of the WX station sensor housings over the next few days. My goal is to have all of the hardware needed 
for a first rendition and possible installation gathered by the end of next week, 2026-04-01. 
