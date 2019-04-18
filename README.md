# SurfacePro3-WifiPSDisable
## Requires iw package
Surface Pro 3 Wifi Powersave Mode Disable systemd entry

This is how to create a custom systemd link to turn off powersaving mode for your wifi card. This can be applied to affect any wifi card, but the purpose of this git is to help Surface Pro 3 linux users who do not want to recompile their kernel with the latest Marvell firmware modules every time the kernel is updated.

Welcome to the readme, this will help guide you to turning off your wireless card's power saving mode to help alleviate any failures to wake up from powersaving mode and causing a non-network state on your machine.

Firstly, we will need to create the systemd link, which is done with:

"sudo systemctl edit --force --full wifipsdisable"

Your text editor should come up to a blank screen.

Then you will enter the following chunk:

---------------------------------------------------
>#Disable wifi power saving mode on Surface Pro 3

>[Unit]

>Description=Turn off Wireless Power Saving Mode

>After=multi-user.target

>[Service]

>Type=oneshot

>ExecStart=/usr/bin/iw dev wlp1s0 set power_save off

>[Install]

>WantedBy=multi-user.target

---------------------------------------------------

Once that is populated, exit and save.

Use "sudo systemctl enable wifipsdisable" to enter your new entry into the systemd boot sequence.

Restart your system and check to see if the changes took effect using
"iw dev wlp1s0 get power_save"
If your terminal returns "Power save:off" you know that your entry is successful and you won't have to manually enter the power save off command any further.


## Comments:
## 1. The service name does not have to be "wifipsdisable" that is only for this readme.
## 2. The After= line does not need to point to multi-user.target, only a point in the sequence in which your network manager has brought up and assigned the wireless interface a name.
## 3. Your wireless interface name may be different from wlp1s0 used in this readme. To find this, run "ip add" and see what your interface's name is.
