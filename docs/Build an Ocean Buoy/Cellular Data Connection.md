---
sidebar_position: 6
---

Working from these instructions: https://dev.blues.io/quickstart/notecard-quickstart/notecard-and-notecarrier-pi/#notecard-quickstart

Once the hardware is connected, I shifted to the Notecard CLI isntructions: https://dev.blues.io/tools-and-sdks/notecard-cli/

This requires installing the package manager `brew` which are found on the Homebrew website (brew.sh). As of this typing, they are:

```python
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Once that installation is done, the command `brew` is still not recognized. The final instructions at the end of the installation show why this is the case:

```python
- Run these commands in your terminal to add Homebrew to your PATH:
    echo >> /home/planetschool/.bashrc
    echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /home/planetschool/.bashrc
    eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
- Install Homebrew's dependencies if you have sudo access:
    sudo apt-get install build-essential
  For more information, see:
    https://docs.brew.sh/Homebrew-on-Linux
- We recommend that you install GCC:
    brew install gcc
```

So I followed these instructions and now `brew` is recognized. Note that the above commands are customized for my Raspberry Pi (i.e. it references “planetschool”). Therefore, the directory path would need to be replaced with your home directory. Or just copy paste the commands that are created after your own brew installation.

With that done, I could finally run `brew install --cask blues/note-cli/note-cli`

Install done! I can test it by running `notecard` as a command in the terminal and seeing a bunch of information on how to use that command. When I run `notecard -info` though, I don’t see any information about my notecard. 

The Notecard Pi uses i2c as its means of communication, so we need to change over to i2c. Switching back to the Notecard-Pi Quickstart guide, I get the next instruction of running `./notecard -interface i2c` . Now when I run `notecard -info` I see the IMEI and other details about my notecard. Nice!

Now I run the `notecard -play` command to enter what they call “Interactive Mode” and send requests to the notecard. Now I can enter the command `{”req”:“card.version”}` to get information about my notecard. Here is what this looks like for me:

```bash
planetschool@raspberrypi:~ $ notecard -play
Welcome to the Notecard interactive REPL.
Type 'help' for a list of commands
>>> {"req":"card.version"}
{
    "board": "5.13",
    "body": {
        "built": "Oct 16 2025 14:29:26",
        "org": "Blues Wireless",
        "product": "Notecard",
        "target": "u5",
        "ver_build": 17434,
        "ver_major": 9,
        "ver_minor": 3,
        "ver_patch": 1,
        "version": "notecard-u5-9.3.1"
    },
    "cell": true,
    "device": "dev:864009066242435",
    "gps": true,
    "name": "Blues Wireless Notecard",
    "ordering_code": "EA0WT1C0AXBN",
    "sku": "NOTE-MBNAN",
    "version": "notecard-9.3.1.17434"
}
>>> exit
```

I use `exit` to end Interactive Mode. Note that I do not need to be in Interactive Mode to send commands to the notecard. To send the same command outside of Interactive Mode, I need to use the `notecard` command plus `-req` each time I send a command and I need to put the command in quotation marks. The information I get back comes as unformatted JSON output. Here is the same command outside of Interactive Mode:

```python
planetschool@raspberrypi:~ $ notecard -req '{"req":"card.version"}'
{"body":{"built":"Oct 16 2025 14:29:26","org":"Blues Wireless","product":"Notecard","target":"u5","ver_build":17434,"ver_major":9,"ver_minor":3,"ver_patch":1,"version":"notecard-u5-9.3.1"},"version":"notecard-9.3.1.17434","name":"Blues Wireless Notecard","device":"dev:864009066242435","sku":"NOTE-MBNAN","ordering_code":"EA0WT1C0AXBN","board":"5.13","cell":true,"gps":true}

```

Alright, we have our notecard working and we can communicate with it. Onto the next step!

## Creating a Notehub Account

No surprises here. I created the account and added my ProjectUID to the Notecard using the `{"req":"hub.set", "product":"com.your-company.your-name:your_product"}` command.

## Completing the Data Pipeline

Now for the grand finale: sending data from the Raspberry Pi to Notehub and then from Notehub to Thingsboard (our dashboard). I followed the instructions [here](https://thingsboard.io/docs/paas/reference/http-api/) for getting a HTTPS path for Notehub to use. This gave me the following address: `https://thingsboard.cloud/api/v1/$ACCESS_TOKEN/telemetry`

Assuming I have an access token that is “al66SHBwYLSHRBuZrRwh” (this is a fake token), then my HTTPS URL would be: `https://thingsboard.cloud/api/v1/al66SHBwYLSHRBuZrRwh/telemetry`

I then went into “Routes” within my Notehub dashboard and clicked “+” to create a new Route, selecting HTTPS. Paste the URL above into the required “URL*” field and name this route “Thingsboard.” Before saving, I went down to “Filters” and selected “data.go” as the only notefiles to be sending. That way I am not using up bandwidth to send system status information that I don’t care about including in my dashboards. Save this Route and make sure it is enabled via the toggle switch above the Route Name.

If we click on “Events” on the sidebar, we can see how much data is being sent from the Notecard that is not actual sensor data. We are only interested in sending data.go to Thingsboard for display in our dashboard.

I then test that this full pipeline is working. In my terminal, I open a new `notebook -play` session (or keep going in your existing session if you didn’t close it. You can tell if you are still in Interactive Mode if you see `>>>`). Using the Notecard “Getting started” instructions, I create a fake piece of data using `{"req":"note.add","body":{"temp":35.5,"humid":56.23}}` then I send it using the `{"req":"hub.sync"}` command. Once I get an empty bracket back indicating success (e.g. `{}`), I can check the “Events” sidebar in Notehub. The row with `data.go` in the “File” column should have a green checkmark next to it in the “Status” column. When I hover over this checkmark, it tells me “All routes successful”.

Let’s confirm this on the Thingsboard side of things. Navigate to your device within [thingsboard.cloud](http://thingsboard.cloud) and click on “Latest Telemetry.” I have found I sometimes need to refresh this page. You should see data like “best_lat” and “best_lon,” which will be useful for tracking the location of our buoy. If you keep looking, you should find the fake data that you sent (`"temp":35.5,"humid":56.23`) next to a key named “body.” 

WELL DONE! You just sent data from your Raspberry Pi over cellular to Notehub and then told Notehub to send it to Thingsboard. This is legit coding work. Take a victory lap and then let’s keep going.