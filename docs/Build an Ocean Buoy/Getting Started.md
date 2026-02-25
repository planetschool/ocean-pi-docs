---
sidebar_position: 2
---

Before you begin this project, please consider completing our Ocean Creature GIF project first, which is a good introduction for Python and Raspberry Pi beginners. We will be building on skills that were introduced in that project.

This project cannot be completed using the Trinket emulator; it requires a physical Raspberry Pi and additional hardware depending on how far you go. We will list all of the hardware we have used in our installations. If you make modifications or additions, we would love to know about how it goes!

# Setup

We will begin this project with a freshly installed version of the latest Raspberry Pi Operating System (OS), which as of this writing is Debian 13 (trixie). If you have not done this, follow the instructions on the [Raspberry Pi website](https://www.raspberrypi.com/software/) for downloading the Raspberry Pi Imager.

Once you have this flashed to a microSD card, boot up your Raspberry Pi for the first time, follow the prompts to configure it, and allow it to install any updates. I always find it funny that a freshly installed latest OS release will immediately have a bunch of updates on first boot, but software moves quickly.

In building this project, we will be creating a virtual environment to build our project. Imagine creating a little laboratory inside of a shipping container—fully self-contained and sealed off from the larger facility it is inside. That is the virtual environment. Any software packages we install, any code we write, any changes we make are isolated from your Raspberry Pi OS.

Before creating your virtual environment, there are a few system-wide packages to install on the OS itself. Open the Terminal and type these commands there:

```bash
sudo apt update
sudo apt install python3-dev python3-pip libffi-dev build-essential libgpiod-dev swig
sudo apt install -y i2c-tools
```

Installing libraries and dependencies (other software that your code will depend on) can feel tedious as you wait for progress bars to make their way to 100%, but view it as assembling your tools, cleaning the shop, and getting ready to build. It is essential.

The last step is to turn on SSH, I2C, SPI, and Serial. You can do this in the Raspberry Pi Main Menu via Menu > Preferences > Control Centre > Interfaces. Your Raspberry Pi will need to restart after this.

**Note:** The home directory on my Raspberry Pi is `planetschool` so if you see that in commands or code, you should replace it with whatever your directory name is. When you set up your Raspberry Pi for the first time, it is whatever you selected for a username.

## Make Your Virtual Environment

Making a virtual environment (or “venv”) is as simple as running the following command in the terminal:

```bash
python3 -m venv venv-ocean-pi --system-site-packages
```

I named my virtual environment `venv-ocean-pi` if you wanted to name your venv “endurance,” you would type `python3 -m venv endurance --system-site-packages` . I like starting my virtual environments with “venv,” which is also convention. Making the venv in your home directory helps with activating it when you boot up your pi—you don’t have to navigate to another folder to activate your venv.

Once your venv is created, you can see it by running (this is a lowercase “L” not a “1”—it stands for “list”) `ls` in terminal to list the files of directories of the folder you are currently inside. When I run `ls` on my Raspberry Pi inside `planetschool`, I see:

```bash
Desktop    Downloads  Pictures  Templates      Videos
Documents  Music      Public    venv-ocean-pi
```

## Activate Your Virtual Environment

To activate your venv, run the following in terminal:

```bash
source venv-ocean-pi/bin/activate
```

You will know your venv is active because the terminal prompt will change and show your venv, letting you know that any terminal commands from this point on are being run inside your sealed off venv area.

Before venv is active:

```bash
planetschool@raspberrypi:~ $
```

After venv is active:

```bash
(venv-ocean-pi) planetschool@raspberrypi:~ $
```

To deactivate your virtual environment, run `deactivate` .

:::tip[Pro tip]

One of the reasons I like prefacing my venv name with `venv` is that as soon as I type “ve” I can hit Tab and the rest of the name will auto-complete. Terminal will auto-fill file or directory names when you hit tab until it hits a point in a name where the name is no longer unique. So if I had two virtual environments in `planetschool` and my other venv was named `venv-planetschool` then hitting Tab after “ve” would complete until `venv-` at that point, it would need more guidance from me on how to finish completing the directory name. So once I type either “p” or “o,” it would be able to complete the rest of the name because I would have steered it towards one filename or the other. Another time-saving hack is using the “Up” arrow in terminal to scroll through previous commands you have typed. This saves re-typing common commands.

:::

## Install Libraries in Your Virtual Environment

With your virtual environment active, run the following commands:

pip3 install gpiozero

```jsx
pip3 install gpiozero pyserial board smbus2 paho-mqtt adafruit-blinka
```

**Note:** If you try and install these packages without your virtual environment activated, you will get an error from your Raspberry Pi: `error: externally-managed-environment` . This error is designed to protect your Raspberry Pi’s system version of Python from being tampered with. Since these packages all come from reputable sources, you could follow the error’s suggestion and add 
`--break-system-packages` to the install command and you would probably be fine. But it is best practice to use a venv and install what you need for a project inside the venv, leaving your system version of Python alone.

**🎉 You Did It!**

## Questions After This Stage

**Will I have to do any of these commands again?**

No. These commands were installations and they are now installed on your Raspberry Pi inside of your virtual environment. Aside from updating them from time to time, you never have to do any of this again.