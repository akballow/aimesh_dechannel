# Warning
> Following instructions are provided without any warranty, and may even get you in trouble legally.
> The instructions are provided for testing, and should be use with care.
> We (including commentators) are not responsible for any damage to your device(s).

# NOTE
You have to be okay with using channel 13 for 2.4ghz and 

# Introduction
One of the biggest pain points of AiMesh is the ineability to change channels for the mesh nodes so for example if your main router is using channel 6 and 36, all the mesh nodes will too. You probablly read online how you can ssh to the mesh node and change the channel but this never ends up working because the mesh nodes always sync with the main router and going back to the main router channels. There is a way to trick it by making your main router do channels that the mesh nodes can not do, when this happens the mesh nodes default to auto channel.

# Instructions
This was tested with a RT-AX86U as the main router and XD5's as the mesh nodes. You do not need firmware which allows jffs script at startup but it helps if you want to preserve settings after a reboot. Also you can not ever make wireless changes in the gui as it will most likley overrite any of the changes you make.

First ssh into your main router and change the country code. This removes all the channel restrictions by picking the wildcard region code so you have access to all regions
```sh
nvram set location_code=#a
nvram commit
service restart_wireless
```

What you can do now is set channe 13 on your main router and reset wifi
```sh
nvram set wl0_chanspec=13
nvram commit
service restart_wireless
```

After doing this you will see your main router be on channel 13, but the mesh nodes which do now have the location code #a set will not be able to set 13 and they will switch to auto picking a random channel. You will have for example All mesh nodes on Channel 1 and the Main router on Channel 13.

Lets say you want to make another mesh node on channel 13 so you have even spread of nodes on channel 1 and 13, simply ssh in to the node and do the same steps.
To make the changes persistant make the above commands in a jffs script in "/jffs/scripts/init-start"
