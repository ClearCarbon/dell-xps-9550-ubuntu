These instructions were written Nov 2016. We are installing Ubuntu 16.10.

* Press F12 to enter BIOS setup
* Secure Boot -> Secure Boot Enable -> Change from "Enabled" to "Disabled"
* System Configuration -> SATA Operation -> Change from "RAID" to "AHCI"
* Apply changes and restart.
* Press F12 to select your USB Stick to Boot.


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

* sudo add-apt-repository ppa:nilarimogard/webupd8
* sudo apt-get update
* sudo apt-get install prime-indicator


## Desktop Swipe
This no longer works after fixing palm detection. I suspect this is because we 
had to blacklist one of the trackpad devices.


If you are used to switching desktops in OS X with 3 finger swipe this provides
a similar functionality for Ubuntu.

* Run `sudo apt-get install touchegg`
* Run `touchegg` and then close to create the default setup
* Open ~/.config/touchegg/touchegg.conf
* Add
  ```
  <gesture type="DRAG" fingers="3" direction="RIGHT">
    <action type="SEND_KEYS">Control+Alt+Right</action>
  </gesture>

  <gesture type="DRAG" fingers="3" direction="LEFT">
    <action type="SEND_KEYS">Control+Alt+Left</action>
  </gesture>

  <gesture type="DRAG" fingers="3" direction="UP">
    <action type="SEND_KEYS">Control+Alt+Up</action>
  </gesture>

  <gesture type="DRAG" fingers="3" direction="DOWN">
    <action type="SEND_KEYS">Control+Alt+Down</action>
  </gesture>
  ```
* Remove any duplicate 3 finger drag commands.
* Edit ~/.xprofile to add
  ```
  synclient ClickFinger3=0
  synclient TapButton3=0

  touchegg &
  ```
* Restart


## Fixing Palm Detection

* Open '/etc/modprobe.d/blacklist.conf'
* Add the line 'blacklist i2c_designware-platform'
* Open '~/.profile'
* Add "xinput set-prop 'SynPS/2 Synaptics TouchPad' 'Synaptics Palm Detection' 1"

Palm detection helps but isn't perfect, especially near the edges, we can improve it
by disabling the touchpad for 300ms while typing. This will not disable a mouse plugged in.

* Launch startup applications and add 'syndaemon -i 0.3 -K -d' as a startup application.
* Restart your laptop again.


## Remaining Problems

* Palm rejection still not working (above solution locks up the mac at times).
* Have to restart X to switch to nvidia graphics.
* Wired networking doesn't work after a suspsend or reconnect of the dongle.
* HDMI on the dongle won't work at anything other than low resolutions.
* Switching Audio Devices doesn't always seem to work well (devices without sound).
