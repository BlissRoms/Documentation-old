
# Troubleshooting

## 32-bit processors only (Intel Atom and similar)

1. Install Android-x86 32-bit OS from https://www.android-x86.org/ (doesn't matter which version, as long as it's 32-bit)
2. Update with Bliss OS 32-bit (current version is 11.9). After reboot you should be able to access the `grub` menu.
3. In `grub` menu select "Debug mode"
4. Run the following commands:
    <pre>```
mount -o remount, rw /mnt
cd /mnt/grub
vi menu.lst
    ```</pre>
5. Add `nomodeset` before every "SCR=**" line. For example, your configuration should look something like this:
    <pre>```
nomodeset
SCR=700
nomodeset
SCR=716
nomodeset
SCR=795
    ```</pre>
6. Press Ctrl+O to save, and then Ctrl+X to close.
7. Type `reboot -f`

You should be finished! If all goes well you will boot into Bliss OS on your 32-bit machine.


## `grub2` kernel parameters and options 

**You will want to pay attention here!** With Bliss OS on the PC, we tend to use quite a few command line options to get things working right. We've gathered a few of them here to explain them a little bit.

[A full list of available kernel parameters can be found here.](https://www.kernel.org/doc/Documentation/admin-guide/kernel-parameters.txt)

Brief options overview:

 - `parameter`: Description
 - `root=`: Root filesystem.
 - `rootflags=`: Root filesystem mount options.
 - `initrd=`: Specify the location of the initial ramdisk.
 - `init=`: Run specified binary instead of `/sbin/init` (symlinked to `systemd` in Arch) as `init` process.
 - `init=/bin/sh`: Boot to shell.
 - `systemd.unit=`: Boot to a specified target.
 - `nomodeset`: Disable kernel mode setting (useful for fixing video driver panics) This will load mostly everything in software rendering/support mode. No hardware acceleration. Good for debugging. 
 - `panic=`: Time before automatic reboot on kernel panic.
 - `debug`: Enable kernel debugging (events log level).
 - `mem=`: Force usage of a specific amount of memory to be used.
 - `maxcpus=`: Maximum number of processors that an SMP kernel will bring up during bootup.
 - `selinux=`: Disable or enable SELinux at boot time.
 - `netdev=`: Network devices parameters.
 - `video=<videosetting>`: Override framebuffer video defaults.
 - `sleep=1`: This will enable the system.prop value for `sleep.earlysuspend=1`, and on some machines, it enables the proper sleep state.
 - `acpi_sleep=s3_bios,s3_mode`: Sometimes needed for older machines to enter sleep mode properly
 - `SETUPWIZARD=0`: This command will skip SetupWizard on boot. (Only needs to be run once!)
 - `AUTO_LOAD=old`: This will load android-x86 variants using the old `modprobe` method to init devices. We sometimes use this to debug devices not starting. 
 - `DEBUG=1 & DEBUG=2`: These enable verbose console debugging, giving another command shell after loading kernel modules, but before Android `init`
 - `vga=xxx & video=`: These are the common video modes that you can boot into if it doesn't pick the best choice automagically. You can also use `video=` as resolution parameters: `video=LVDS-1:d video=1366x800`. [Learn more from our own Henri Koivuneva!](https://groups.google.com/forum/#!msg/android-x86/jSF3RnADnqA/1sfYdGV_AQAJ)
 - `HWACCELL=1`: This will disable graphics hardware acceleration, enabling rendering through Swiftshader. (Must use this if running headless)
 - `buildvariant=eng, user, userdebug`: This is the commandline parameter to run the current build as `eng`, `userdebug`, or `user` 
 - `DPI=xxx`: This will manually set the DPI on init. Use this if things are too big/small for you.
 - `fbcon=variablename`: This is to configure framebuffer to use various options. Usually used to help fix video settings, etc. Even default rotation on some Atom tablets. Example: `video=efifb fbcon=rotate:1`
 - `VULKAN=1`: Required for Vulkan-supported chipsets. This enables `hwcomposer` to work right with screenshots and other things.

As an example, here are a few of the boot options used in testing:

    menuentry 'Bliss-x86 Test-Oreo' --class bliss {
        search --file --no-floppy --set=root /AndroidOS/android.boot
        linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS androidboot.selinux=permissive androidboot.hardware=android_x86_64 buildvariant=eng quiet sleep.earlysuspend=2 DATA=
        initrd /AndroidOS/initrd.img
    }

    menuentry 'Bliss-x86 Test-Oreo AUTO_LOAD=old' --class bliss {
        search --file --no-floppy --set=root /AndroidOS/android.boot
        linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS androidboot.selinux=permissive androidboot.hardware=android_x86_64 buildvariant=eng quiet DATA= AUTO_LOAD=old
        initrd /AndroidOS/initrd.img
    }

    menuentry 'Bliss-x86 Test-Oreo - SETUP_WIZARD=0' --class bliss {
        search --file --no-floppy --set=root /AndroidOS/android.boot
        linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS androidboot.selinux=permissive buildvariant=eng SETUPWIZARD=0 quiet DATA=
        initrd /AndroidOS/initrd.img
    }

    menuentry 'Bliss-x86 Test-Oreo - debug=1' --class bliss {
        search --file --no-floppy --set=root /AndroidOS/android.boot
        linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS androidboot.selinux=permissive androidboot.hardware=android_x86_64 buildvariant=eng SETUPWIZARD=0 quiet DATA= DEBUG=1
        initrd /AndroidOS/initrd.img
    }

    menuentry 'Bliss-x86 Test-Oreo - debug=2' --class bliss {
        search --file --no-floppy --set=root /AndroidOS/android.boot
        linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS androidboot.selinux=permissive androidboot.hardware=android_x86_64 buildvariant=eng SETUPWIZARD=0 quiet DATA= DEBUG=2
        initrd /AndroidOS/initrd.img
    }
 
