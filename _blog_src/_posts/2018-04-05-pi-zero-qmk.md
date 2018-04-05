---
layout: post
title: Pi Zero W + iPad, flashing QMK with a Pi, and some musings
---

## Background
One of the best things about trying to use an iPad seriously as a daily driver machine is coming up with fun ways around the arbitrary limitations Apple sets up in iOS. Being a mechanical keyboard enthusiast myself, I usually prefer to use my iPad with one of my portable keyboards (the KPRepublic JJ40 or my Happy Hacking Keyboard Professional 2[^1]), and I run QMK, the Quantum Mechanical Keyboard firmware on them.

It's a well known fact that Apple keeps its iOS platform locked down tight - back then running unsigned code if you don't have a jailbreak was impossible (since [Xcode 7](https://www.lifehacker.com.au/2015/12/how-to-install-unapproved-apps-on-an-iphone-without-jailbreaking/) one could self-sign applications to run on the iPad, although that involves renewing the signing certificate every few months). Even if you could somehow get `gcc-avr` and the dependencies installed somehow, compiled a `.hex` to flash, the iPad doesn't allow apps any access to the USB port on the Camera Connection Kit (CCK) - so no luck there.

I've toyed with the idea of having a Raspberry Pi along with my iPad to somewhat replicate my iPad + VPS setup several times before so I have access to a (relatively) full-fledged computing environment. The iPad fares just fine for editing code and taking notes (although a larger screen than the 9.7" one on my Pro would be swell), but it's really dependent on having a good internet connection to access and control my VPS. Add to that my experience of accidentally flashing the wrong file on my keyboard, leaving me keyboard-less on my iPad until I got home to my laptop.

[^1]: using the alt controller by Hasu so it could run QMK, itself a fork of TMK. It's not as much the programmability I needed at first, but the fact that the stock HHKB will not work with the iOS Camera Connection Kit because it reports a power draw that is too high (>100 mA) due to the fact that it has a USB hub built-in.

## The setup
Enter the Raspberry Pi Zero W - at a low price of $10 for one (and after shelling out some more for the official case) this diminutive device has pretty much the same specifications as the original Raspberry Pi which has come to be loved by many. The W stands for Wireless, which means it has WiFi and Bluetooth wireless radios built-in, much like the Model 3B.

After an install of Raspbian Stretch, I followed [this](http://www.40percent.club/2017/10/pi-zero-tmk-isp.html) guide to set up QMK on my iPad, but with the Debian instructions to [set up the build environment](https://github.com/qmk/qmk_firmware/blob/master/docs/getting_started_build_tools.md) from the original guide instead of the `sudo apt-get install` command in that blog post.

A `git clone` of the original QMK repo later, I proceeded to try to compile a keymap - success, although it seemed to take closer to a minute to compile instead of the ~10s it usually takes on my Mac.

### dfu-programmer for flashing PS2AVRGB boards
For a typical atmega32u4 board running the Caterina or DFU bootloaders, the required tools for flashing the boards should already be installed after following the "setting up the build environment" guide above. Follow the respective instructions from QMK for your keyboard.

The bootloader and flashing tool used for the PS2AVRGB (atmega32a) boards, however, isn't installed by the `sudo apt-get install` command. The PS2AVRGB (base/board) repos also lacked instructions for setting up `dfu-programmer` on Linux, with the instructions pointing to using `brew` and installing `dfu-programmer` from a Homebrew tap (macOS-specific).

A bit of sleuthing brought up [this Reddit post](https://github.com/qmk/qmk_firmware/blob/master/docs/getting_started_build_tools.md) with instructions on how to compile BootloadHID (source code link [here](https://www.obdev.at/downloads/vusb/bootloadHID.2012-12-08.tar.gz)). Just install <!--pyusb (`pip install pyusb`) and -->the `libusb-dev` dependency using `sudo apt-get install libusb-dev` before compiling bootloadHID, then copy the compiled binary to `/usr/bin` after compilation so it is accessible from anywhere (alternatively you can simply add the directory with the bootloadHID binary to `PATH`).

The linked guide also shows how to put the board into bootloader mode, but I simply manually booted my JJ40 into bootloader mode (hold backspace while plugging in - may differ between boards).

### Raspberry Pi as an Access Point
Follow [this guide](https://www.raspberrypi.org/documentation/configuration/wireless/access-point.md) on the Raspberry Pi website to set it up as an access point.

After the setup, connect the iPad to the WiFi network you configured earlier. The Pi would then be accessible at `192.168.4.1` once you've connected to the Pi's network.

### Enabling SSH, VNC
Use `raspi-config` to enable SSH and VNC. SSH enables us to remotely control a secure shell on the Pi, and VNC lets us remotely control the Pi's GUI (if the Pi is running a version of Raspbian with a GUI). I used [VNC Viewer](https://itunes.apple.com/au/app/vnc-viewer-remote-desktop/id352019548), although you could choose from the excellent wealth of VNC client applications (Screens is a good recommendation from many).

### Android and other platforms
These instructions are platform-independent: you could SSH (and/or VNC) into your Raspberry Pi with the respective tools on any platform. On Android, you could set up [Termux](https://termux.com/) and install `ssh`, then connect to the Pi's wireless network and ssh the pi to compile and flash the firmware. On Windows we have PuTTY, and on macOS or Linux, `ssh` in the built-in Terminal emulators would work.

## Thoughts on the Pi Zero
The size and weight is quite impressive - it actually fits in one of my IEM cases.

One of the things you'll be compromising on is on port selection though: compared to my regular Pi Model 3B, the Zero only has one single microUSB for USB on-the-go, requiring a dongle/adapter to get a full-sized USB port. The HDMI port on the Zero is also of the miniHDMI variety: as I did not have a miniHDMI to HDMI cable or adapter I simply did the entire setup process on my 3B and swapped the SD card into the Zero afterwards. Stunningly everything worked, except for the fact that my right-angled USB on-the-go adapter wouldn't fit due to it blocking the microUSB power in port. And like the (frustrating) trend in smartphones, it no longer has a 3.5mm jack.

Although for the price, size, I don't have too many complaints either way.
