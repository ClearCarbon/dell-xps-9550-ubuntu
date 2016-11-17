These instructions were written Nov 2016. We are installing Ubuntu 16.10.
They assume you are completely wiping any Windows installation and just
installing Ubuntu.

Pull requests are very welcome.

* Press F12 to enter BIOS setup
* Secure Boot -> Secure Boot Enable -> Change from "Enabled" to "Disabled"
* System Configuration -> SATA Operation -> Change from "RAID" to "AHCI"
* Apply changes and restart.
* Press F12 to select your USB Stick to Boot.
* Install Ubuntu.


## Switch to Intel Graphics (8+ hours battery life)

* Run 'Additional Drivers' from the Ubuntu Search
* Switch to running the Nvidia Binary Driver (tested)
* Restart your machine
* Run 'NVIDIA X Server Settings' from the Ubuntu Search
* In Prime Profiles switch to 'Intel Power Saving Mode'
* Log out and back in.

### Install Prime Indicator

This step is optional, but if you will be switching graphics cards regularly
pretty useful. It places an indicator in your system try to easily switch
between the nVidia and Intel cards.

* `sudo add-apt-repository ppa:nilarimogard/webupd8`
* `sudo apt-get update`
* `sudo apt-get install prime-indicator`


## Update BIOS

This seems to help with a few little niggles, including multi screen use.

* Download the latest BIOS update from (Dell)[http://www.dell.com/support/home/us/en/19/product-support/product/xps-15-9550-laptop/drivers]
* Run `sudo cp <bios.exe> /boot/efi`
* Reboot your machine pressing F12 to enter boot selection
* Select the BIOS Update option
* Select the bios.exe you copied earlier
* Follow instructions.

## Fixing Palm Detection

Warning: The below fixes palm detection however the mouse may freeze during use or after
a sleep.

* Open `/etc/modprobe.d/blacklist.conf`
* Add the line `blacklist i2c_designware-platform`
* Open `~/.profile`
* Add `xinput set-prop 'SynPS/2 Synaptics TouchPad' 'Synaptics Palm Detection' 1`

Palm detection helps but isn't perfect, especially near the edges, we can improve it
by disabling the touchpad for 300ms while typing. This will not disable a mouse plugged in.

* Launch startup applications and add `syndaemon -i 0.3 -K -d` as a startup application.
* Restart your laptop again.


## Remaining Problems

* Palm rejection still not perfect, seems to detect palm more on the edges of the trackpad.
* Wired networking doesn't work after a suspsend or reconnect of the dongle.
* HDMI on the dongle won't work at anything other than low resolutions.
* USB-C to Thunderbolt doesn't work at all.
* Switching Audio Devices doesn't always seem to work well (devices without sound).
* Have to restart X to switch to nvidia graphics.
