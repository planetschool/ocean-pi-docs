---
slug: weather-station-bugs
title: Weather Station Bugs
authors: aram
---

This is my progress update for the work so far. I definitely have had some trouble with the Wx camera but I think it's close. 

So far, my progress for the project has covered linking my Raspberry Pi 500 to the weather station array. I quickly switched back to the RPI5 that will be installed with the station and ran the code because it is simpler to work through the software from there. I am still running into an issue within the automated activation sequence. When the Raspberry Pi powers on, the issue appears to be that the code starts running automatically when the sensors and the Raspberry Pi get power. The code encounters an error at the camera activation step because it reports that the camera is already running or is being accessed by another function. I am troubleshooting how to stop the background function from accessing the camera so it can be used by my code and run smoothly. Our plan is to remove the automatic start-up section of the software and just remotely start the weather station and buoy from a desktop unit once the first rendition is installed. The research buoy is nearly operational; the last pieces are to complete the first waterproof model and decide where it will be installed. 