---
slug: sensehat-on-wonder
title: Sense HAT Installed on Wonder
authors: thane
---

After a visit to Key West, I was able to get the Sense HAT Station on the deck of the ship successfully broadcasting video to our Thingsboard dashboard. This Sense HAT Station is in a fully waterproof container on deck, so the humidity and temperature sensors will not work. This case was always meant to be a temporary prototype and it is now successfully deployed.

<!-- truncate -->

This deployment revealed a work item that needs to be added to the list: local broadcasting of data with remote broadcasting (i.e. thingsboard) as an optional extension. Building any of our projects on top of a service that requires payment is not ideal. The baseline should have any one of our weather stations 1) broadcasting to a local screen for display in a classroom, for example. 2) sharing data with the larger Ocean Pi project network, which would be to a platform that we pay for. 3) methodology for adding thingsboard if desired.
This week I was also able to make meaningful forward progress on the Ocean Pi website. For 2024-2025, oceanpi.org redirected to planetschool.org/ocean-pi. This format restricts Ocean Pi from being able to have its own standalone identity, which I believe is important for the project’s success.
Next steps:
-	Finish first draft of oceanpi.org (next week)
-	Create sketch of prototype Ocean Buoy platorm (next week)
-	Create, test, and deploy thingsboard dashboard for all Ocean Buoy datapoints (next week)
-	Fully wire up and test battery, cellular, and solar panel components and order any missing equipment (next week).
Next project idea for class:
-	Create an Astro Pi replica that continuously streams its temperature and humiditiy data to thingsboard and activates its camera to stream for 15 seconds when it senses that the door it is mounted on opens or closes.
Ocean Pi Challenge 2026:
-	Using any of the data points tracked aboard Wonder, guess which body of water Wonder is in when your code is run for 30 seconds at a random point in our journey. The bodies of water that you can choose from are 1) Atlantic Ocean in the Gulf Stream, 2) Atlantic Ocean NOT in the Gulf Stream, 3) Intracoastal Waterway of North Carolina, 4) New York Harbor and Long Island Sound, 5) Block Island Sound and Buzzards Bay. For an added challenge, you can try and cross-reference data from Wonder with external weather data to make a guess at the ship’s latitude and longitude.
