# slim-pro-9
Fixes and patches for Linux on the Lenovo Slim Pro 9 16IRP8

# Audio
To make the subwoofer's work you need the following:
#### Disable driver runtime suspend/resume PM
as root (if rc.local doesn't exit - create, make executable and add `#!/bin/sh`)
```
cat << EOF >> /etc/rc.local
# Disable runtime suspend/resume for tas 2781 as it is not working
echo on > /sys/bus/i2c/drivers/tas2781-hda/i2c-TIAS2781\:00/power/control
EOF
```
On Fedora the rc.local file is `/etc/rc.d/rc.local`

#### Place the firmware in /lib/firmware
The files `TIAS2781RCA4.bin` and `TAS2XXX387D.bin` need to be copies in `/lib/firmware`

#### Controling subwoofer speakers volume
By default the normal speaker volume controls don't control the volume of the subwoofer's. 
The 2781 speakers are controlled with a separate control
```
amixer cget numid=3 -c 0
numid=3,iface=CARD,name='Speaker Digital Gain'
  ; type=INTEGER,access=rw---R--,values=1,min=0,max=200,step=0
  : values=148
  | dBscale-min=-100.00dB,step=1.00dB,mute=0
```
To make the amp speakers change volume in sync with the volume of the main speakers - use https://github.com/darinpp/alsa-controller
It will update the digital gain for the amp speakers when the main speaker's volume changes.
There is an included config for the alsa-controller service with the correct source and destination included.

# Keyboard
to be added
i8042 dumbkbd is needed
caps and numlock indicators will not work
