# Installation Guide (Surface devices)

This guide is applicable only to Surface devices.

Reference posts [here](https://forum.xda-developers.com/showpost.php?p=78015375&postcount=76) and [here](https://forum.xda-developers.com/showpost.php?p=76896155&postcount=2107).

Do you have a Surface device that you would like to run Bliss OS on? You're in luck, because Bliss OS was primarily developed and tested on a Surface Pro 3 back on Android Nougat.

Most Surface devices with IPTS require a specific set of firmware for proper functioning. For all other Surface models, the user must upgrade their own firmware, because we can't build for all Surface models without having those devices to test with.
 
To start, here is the series classification for Surface devices:

 - Series 5 devices:
   - Surface Book 2
   - Surface Pro 2017
 - Series 4 devices
   - Surface Book
   - Surface Pro 4
   - Surface Laptop
 - Series 3 devices:
   - Surface Pro 3

For the ipts_firmware files (series 4/5 devices only), please select the correct version for your device:
 
 - `v76` for the Surface Book
 - `v78` for the Surface Pro 4
 - `v79` for the Surface Laptop
 - `v101` for Surface Book 2 15"
 - `v102` for the Surface Pro 2017
 - `v137` for the Surface Book 2 13"

For the i915_firmware files (series 3/4/5 devices), please select the correct version for your device:
 
 - `kbl` for series 5 devices
 - `skl` for series 4 devices
 - `bxt` for series 3 devices
 
All [firmware files can be found here.](https://github.com/jakeday/linux-surface/tree/master/firmware)
 
For Surface Go users, you will have to remove some files and replace them with @jakeday's firmware. Please [see this thread on Reddit](https://www.reddit.com/r/SurfaceLinux/comments/9t53gq/wifi_fixed_on_surface_go_ubuntu_1810/) for detailed information.

Once you have the right firmware you need, you should copy it to a folder on your Bliss install (`SD_card_root/surface`), making sure to put the right firmware in the right folders:

 - IPTS firmware: `SD_card_root/surface/intel/ipts`
 - `ath10k` firmware (some models): `SD_card_root/surface/ath10k`
 - `mrvl` firmware (some models): `SD_card_root/surface/mrvl`
 - `mwlwifi` firmware (some models): `SD_card_root/surface/mwlwifi`
 - `nvidia` firmware for Surface Book 2: `SD_card_root/surface/nvidia`
 
Next, open a terminal (we include one you can enable in "Developer Options"). In the terminal, enter the following commands, giving permission to the superuser popup dialog when prompted:

    su
    mv -f SD_card_root/surface/* system/lib/firmware/

Then you can restart:

    reboot

It should recognize and load the correct firmware versions for your device upon reboot if you did everything correctly.