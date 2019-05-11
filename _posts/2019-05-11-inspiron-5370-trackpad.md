---
layout: post
title:  "Setting up Inspiron 5370 Touchpad gestures on Linux"
date:   2019-05-11 16:53:05 +0530
categories: jekyll update
---

Remove synaptics trackpad driver

`sudo apt remove xserver-xorg-input-synaptics-hwe-18.04`

Install libinput driver

`sudo apt install xserver-xorg-input-libinput-hwe-18.04`

Edit the config file to use libinput driver

`sudo nano /usr/share/X11/xorg.conf.d/*-libinput.conf`

It should look something like this

```
Section "InputClass"
        Identifier "libinput touchpad catchall"
        MatchIsTouchpad "on"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
        Option "Tapping" "True"
        Option "NaturalScrolling" "True"
EndSection
```

Now time to install **libinput-gestures**

```
git clone http://github.com/bulletmark/libinput-gestures
cd libinput-gestures
sudo make install
sudo apt install libinput-tools xdotool
sudo gpasswd -a $USER input # adding yourself to input group to access touchpad
libinput-gestures-setup autostart # make it start on boot
```

Configure the gestures by editing **~/.config/libinput-gestures.conf**

```
# Move to workspace up/down/left/right
gesture swipe up    4    xdotool key ctrl+alt+Down     
gesture swipe down  4    xdotool key ctrl+alt+Up  
gesture swipe left  4    xdotool key super+Left
gesture swipe right 4    xdotool key super+Right   

# Spreads all windows in all workspaces + Show/Hide desktop
gesture swipe up    3    xdotool key shift+super+w
gesture swipe down  3    xdotool key Super_L+d

# Page back and forward on most Web Browsers
gesture swipe left  3    xdotool key alt+Right
gesture swipe right 3    xdotool key alt+Left 

# Maximize + Unmaximize/Minimize gestures
gesture pinch out        xdotool key super+Up
gesture pinch in         xdotool key super+Down
```




