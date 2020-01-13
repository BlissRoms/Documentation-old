
# Extras

## Setting up Taskbar on Bliss OS (Pie)

If you would like to use Taskbar as your default launcher, you will need to first go into "Settings > Blissify > Gestures", and enable something like Carbon Gestures (we recommend setting up three-finger swipes: Right for Back, Down for Home, & Up for Recents), then you can go to "Blissify > Buttons" and switch the navigation mode to SmartBar, then go back and disable the navigation bar from there by switching off "Allow Navigation Bar on the top". At this point, you need to switch to your home screen, so swipe up with your gesture, or tap the Windows key (or Windows-Esc). Then launch Taskbar, enable it, set to launch on boot. We recommend disabling hiding. Enable a couple other settings in the "Freeform" and "Advanced" screens as required. Setting it up this way will prevent any crashes from happening on initial launch. And it allows you to also use the Quickstep launcher as the main background. 

Here's a video tutorial on how to do it properly:
 
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/htFC8poBEPY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Setting up Gapps

[See this thread from @wrwolf2!](https://forum.xda-developers.com/showpost.php?p=79289406&postcount=632)

## Watching Netflix - FOSS & GMS Builds 

Netflix considers our rooted OS as an "incompatible" device, according to their support articles. [This version of Netflix seems to work great](https://www.apkmirror.com/apk/netflix-inc/netflix/netflix-4-16-1-build-15145-release/), as long as you don't update it. If it prompts you, click on "Cancel".


## Setting up Magisk

1. Extract the latest Bliss OS `.iso`, and grab those files:
    * `initrd.img`
    * `ramdisk.img`
    * `kernel`
2. Run:
    `mkbootimg --kernel kernel --ramdisk ramdisk.img --second initrd.img --output boot.img`
3. Copy the boot image to Bliss OS.
4. Use Magisk Manager to patch the boot image. Select Install > Select and Patch File, and then select the `boot.img` you created earlier. The patcher should produce the file `magisk_patched.img`.
5. We need to remove the current superuser binary. Run within Bliss OS in a terminal emulator

    `su`

    `cd /system/xbin && mv su su.bak`

    `exit`

6. Copy the patched `magisk_patched.img` file back to your computer.
7. Unpack the image:

    `unpackimg magisk_patched.img`

8. Rename the following files:

    - `magisk_patched.img-zImage` to `kernel`
    - `magisk_patched.img-second` to `initrd.img`
    - `magisk_patched.img-ramdisk.cpio.gz` to `ramdisk.img`

9. Replace the files back to the original Bliss OS `.iso`.
10. Boot to Bliss, and you should have a successful Magisk installation!
