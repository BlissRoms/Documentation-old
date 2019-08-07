The Instructions
**These instructions are based on 
the Android-x86 project's installation guide. We have not changed the installer, so all actions still apply. Also thanks to @bg260 for his contributions, this guide was adapted partially from his work**
 
*** Team Bliss will not accept any responsibility for users who have not read or understand the instructions, or any damage done to user machines due to lack of understanding all risks involved. You accept all responsibility by continuing beyond this point. *** 
 
*** Any questions, install issues, bug reports, etc will be delightfully ignored unless accompanied with a log, device info, build info, install method, and any other information required to diagnose your issue as NOT user error *** 
 
 
These instructions have changed quite a bit for Android Pie, so consider this section a work in progress. Thank You
 
Bootable USB Install For Bliss OS 11.x - MBR/UEFI/ESP (32/64bit) 
** This is the current recommended method **
 
The gist of it all:
Download the ISO file
Use Rufus or similar to burn to USB drive
Disable Secure Boot, Bitlocker, and any other boot security
Boot into the USB drive.
Run Bliss OS in Live mode to test things out, if all is well, continue to next step
Boot into the USB drive, and choose Bliss OS Install
Pick your poison, but please do this with caution, making sure to fully understand what you are doing.
 
The Detailed bits:
Part 1 - Gather Your Tools
 
** Please note that our Bliss-x86 builds do not currently support this install method for all machines **
 
For this method, we are going to want to download Rufus, and the 32bit .iso or 64bit .iso/.img file of Bliss-x86. And you are going to need a decent speed USB drive (4gb or larger is recommended). Once we have those tools, we can move on. 
 
Part 2 - Flashing Bliss-x86 to the USB drive
 
Plug in your USB drive, and load up Rufus. Once loaded, click on the icon next to the ISO Image dropdown menu. Now browse to where you have your Bliss-x86 (32bit) .ISO, or your Bliss-x86_64 (64bit) .ISO/.IMG file. Once chosen, the dropdown should switch to the correct image type, and fill the rest in for you. Once you are ready, click Start.
 
Part 3 - Testing Bliss on your system !!IMPORTANT STEP!!
### If you as a user do not test the OS first to make sure it is compatible with your device, please do not expect us to support you if you happen to just install it and something goes wrong. You continued to scroll past all of our warnings about reading and understanding what you are doing, so it's all on you###
 
From here, you can choose to reboot your machine, and make sure it can boot to USB from BIOS. Once that is set, reboot and choose the USB. If everything went smoothly on the install process, you should see a Grub boot screen. Select the "Live CD" option, and if your machine is compatible, you should then see a little bit of text, and then the Bliss bootanimation. This will go on for a few minutes, but should eventually boot to Bliss-x86. If the system never boots to Bliss-x86, this is a good sign that your system might not be able to run it. If it does boot, and you would like to install it, continue to the next step.
 
Part 3.5 - Using Bliss-x86 from your USB drive
 
If you so choose to use Bliss from the USB drive, your data will be saved in a temporary state unless you create a data.img to store the data. We can create a data.img in the root dir of the USB drive (make sure you have a minimum 4-5gb free). We suggest using a tool like one from XDA called RMXtools to create it (we suggest you use version 1.7). Check the tool's thread for how to use it, but when you figure it out, you will want to create your data.img inside the root directory of your USB drive, with all the other .img files. From there, just boot into live mode, setup your system the way you want. and the data should be persistant across a reboot now.
 
Part 4 - Setting up and Installing Bliss-x86 on your HDD/SSD/SDcard
 
***Team Bliss is not responsible for any damage, tears, lost time, broken marriages, hallucinations or anything of the sort if things go south with this install. Don't even think about blaming us. You automatically agree to these terms upon continuing the install.***
 
This is where things start to get a little tricky, especially with how PC's vary. Make sure you have a backup plan in case something goes wrong.
 
Start off by opening your favorite Partition Management software, and create a new partition, making it the size you want (suggested minimum is 8gb.). Just format it to NTFS for now, because it will be changed by the installer later anyways. Remember what drive you setup here, it's important. For Windows machines, it will typically be Sda4 or Sda5. Also create another 300mb FAT32 partition for Grub to install to. (This part might require a third-party partition manager, Windows disk manager won't let it be that small)
Boot up the Bliss-x86 USB, and select the Installation option from Grub. (second one down)
The installer will load, and you will have an option to choose which partition you created earlier. Pick it, and select Ext4. ***You don't want to get this step wrong. If you are unsure, please boot back to Windows, and write it down this time. It will be Sd** typically.***
When it asks if you want to install System as R/W, select YES.
When it asks if you want to install Grub, select Grub for Legacy BIOS boot type, Grub2 for UEFI boot type, or neither if you are already running a Linux system.
If you chose to install a Grub option, the installer will allow you to choose. Make sure you select the 300mb partition you setup earlier for Grub.
The process will install and create the data directory/img, so go get a drink or something and come back to it.
When finished, the installer will then ask if you want to run Android-x86, you can just reboot here, and make sure you remove the USB drive.
 
If we have followed all the directions correctly, you should be presented with a Grub boot menu. You can choose your bliss_android_x86 option (or android-x86), and it will boot into Bliss-x86. If you feel the need to customize your grub boot entry, please search the web first. We use the same grub setup that Android-x86 project uses. so their forums will contain just about all the info you will need. 
 
 
Windows based Installer For Bliss OS 11.x - UEFI/ESP (64bit) - (Method no longer supported due to too many people not understanding computer basics)
 
 
** This method might be the easiest currently **
For the overall instructions on using this method, please refer to the tools original thread: https://forum.xda-developers.com/and...-uefi-t3222483
I have taken some time to update the tool for easy install on UEFI/ESP machines. The builds I produce can be found here: 
https://github.com/BlissRoms-x86/And...ree/master/bin 
And the source for those builds can be found here: https://github.com/BlissRoms-x86/And...er-for-Windows
This tool should work on RemixOS as well, but I have not tested it yet (been too busy on this project)
 
Part 1 - Using the Installer
 
The installer has been updated, and it will accept the .iso files for our 8.x/10.x/11.x releases. Just follow the prompts the installer gives. Refer to the orig thread for any questions, and please search before asking. 
!!WARNING!! For Pie, you will need to add "androidboot.hardware=android_x86_64" to the grub entry in order to boot
 
Part 2- Switching the UEFI/EFS boot entry
 
(Option 1) Open the EasyUEFI tool (Search google for it) then switch the UEFI/EFI entry it created to boot first. Close and reboot. 
(Option 2) Use your Bios to select the added UEFI boot entry
 
 
 
How to "prep" a USB using syslinux EFI to run Bliss 7.x/10.x/11.x 
Thanks to @IcedCube
 
Quote:
Originally Posted by IcedCube 
For those who are a little too bleeding edge and like to adventure outside the recommended method that @electrikjesusrecommends, here's how to "prep" a USB using syslinux EFI to run Bliss 7.x/10.x.
Also, I'd appreciate it if he could link it in the first post as a "experimental syslinux EFI" method, because this is what I recommend if some Chinese tablets don't want to boot grub.
 
DO NOT BLAME HIM IF YOUR DEVICE CATCHES FIRE AFTER DOING THIS. BLAME ME INSTEAD.
 
I strongly recommend using a Linux VM or a Linux box for this. Ensure you have the latest version of unsquashfs (part of squashfs-tools) too. Grab the latest build of Bliss x86 7.x/10.x/11.x before continuing!
Grab the ZIP file from my original post, https://forum.xda-developers.com/showpost.php?p=74977694, and extract it to the root of your USB drive. This will bootstrap syslinux EFI onto it.
Make a folder, if you haven't already done so, called "android".
Now, open up the ISO in an archiver. Extract from the root directory of the ISO image the following to your USB drive's "android" folder: initrd.img, ramdisk.img, kernel.
Extract system.sfs to a folder somewhere, maybe in /tmp.
Open a terminal and change directory (using 'cd') to /tmp. Run 'ls' and confirm you see system.sfs shown in the file list. If you get no output, start over as you misplaced a file.
Code:
cd /tmp && ls -al  system.sfs
Run the following code:
Code:
unsquashfs ./system.sfs
This will make a new directory called "squashfs_root".
Bliss 7.x users, this is important: If you are using Bliss 10.x then skip just this step. Change directory to squashfs_root and run a 'ls'. You should have only one file, a system.img inside that directory. Copy that file to your USB's "android" folder.
Bliss 10.x users, this is important: If you are using Bliss 7.x then skip just this step. If you take a look inside squashfs_root, you will notice it's a complete android root filesystem. What we need to do is to move the stuff into a system image. The following will make a 2GB system.img file, format it, mount it and copy the contents of the extracted squashfs into that new disk image.

Code:
mkdir /mnt/tempMount
truncate /tmp/system.img --size=2G
mkfs.ext4 -m0 /tmp/system.img
sudo mount -o loop /tmp/system.img /mnt/tempMount
sudo cp -prv /tmp/squashfs_root/* /mnt/tempMount/
sync
sudo umount /mnt/tempMount
The sync process might take some time. Now copy the /tmp/system.img file to your USB's android folder.
Alright, now that's the system image done. Now you need to make a data image. That's easier than system image. First, find where your USB drive is mounted, it might be at "/media/icedcube/DROIDUSB" or something and cd to the android folder on it:
Code:
cd  /media/icedcube/DROIDUSB/android
. If you're using Ubuntu or any other good distro and have a "Open location in Terminal" option in your File Manager, use that as a shortcut. Now run these commands to make a 3GB data image file - you could try with 4GB but FAT32 maxes out at 4GB per file and I prefer using FAT32 as I'm not sure if the kernel supports exFAT or NTFS properly.

Code:
truncate data.img --size=3G
mkfs.ext4 -m0 data.img
sync
This will be an completely empty ext4 disk image, but will be enough to kickstart Bliss.
Finally, check to ensure everything is in check like so:

Code:
<ROOT>
- syslinux.cfg
- android/
-- kernel
-- system.img
-- data.img
-- ramdisk.img
-- initrd.img
- EFI/
-- BOOT/
--- bootia32.efi
--- bootx64.efi
--- ldlinux.e32
--- ldlinux.e64
Need to add some kernel parameters? Don't panic. Just open syslinux.cfg and add them onto the append before the "initrd=/android/initrd.img" statement.
Unmount the USB from your computer. Carefully plug it into your tablet or laptop and use the BIOS to boot UEFI from USB Drive, partition 1. If all goes well, you will get a black screen with small white text saying "Booting Android..." followed by loading files. You should get the Linux kernel text, then see the Bliss Oreo animation play after a few seconds/minutes depending on your USB drive read/write speed.
 
 
Custom Install For Bliss OS 8.x/10.x/11.x UEFI/ESP (64bit)
 
 
***Again, Team Bliss is not responsible for any damage, tears, lost time, alien abductions, experimental relationships or anything else if things go south with this install. Don't even think about blaming us. You automatically agree to these terms upon continuing the install.***
 
Part 1 - Mounting Your UEFI/ESP Partition
 
You will want to make sure you can view hidden and system files in Explorer options (if you need to , google it), Once you do that, hit the start menu, and type in CMD, and then right click, and open as administrator. It should look like the window image attached to this post.
Once that is open, type in:
 
Code:
mountvol X: /S
Then check to see if it is mounted already
Start Task Manager; a) CTRL+ALT+DEL -&gt; Task Manager b) CTRL+Shift+ESC c) Right click the taskbar and select Task manager.
Click "File" tab -&gt; "Run new task" -&gt; "Browse" -&gt; "This computer" -&gt; SYSTEM (X or type in "x:" in the filepath bar"
 
 
If you cannot access X:, then that could mean one of three things. 1) You have an ESP setup, and just need to scroll down to the ESP System Partition setups section, or 2) You have a legacy MBR setup and just don't know it. or 3) Your setup falls within the other category. Check below for some insight, or the second post for more links to help you figure things out. 
 
ESP System Partition setups 
 
Windows 10 has EFI partition sometimes already mounted under Z: letter, but it's hidden. 
 
A very quick and easy way to access ESP (EFI System Partition) in Windows 10: (no command line use needed!)
Start Task Manager; a) CTRL+ALT+DEL -&gt; Task Manager b) CTRL+Shift+ESC c) Right click the taskbar and select Task manager.
Click "File" tab -&gt; "Run new task" -&gt; "Browse" -&gt; "This computer" -&gt; SYSTEM (Z or type in "z:" in the filepath bar"
Now go to boot/grub/grub.cfg and edit it accordingly with Notepad++ or other editor
Save the file and your're ready to go
 
 
If this still doesn't work - try this:
Run CMD.exe as Admin &lt;- IMPORTANT Then enter following commands:
Code:
taskkill /im explorer.exe /f
This will kill explorer.exe process - don't be surprised It's needed, because by default it's ran by "currently logged in user" and it has to be run as Administrator in order to view the mounted system drive. Administrator account is not the same as an account with administrative privileges.
Code:
Code:
mountvol X: /s
This will mount the system partition that usually consists of uefi related files. X: is the letter of the drive - you can use whatever letter you want, but it has to be free.
Then type:

Code:
explorer
This will run explorer as Administrator and will allow you to browse the mounted system partition.
 
The above may not work for all devices, as some handle UEFI differently.
 
 
 
Part 2 - Run Explorer as Admin
Run CMD.exe as Admin &lt;- IMPORTANT and enter following commands:
Code:
taskkill /im explorer.exe /f
This will kill explorer.exe process - don't be surprised It's needed, because by default it's ran by "currently logged in user" and it has to be run as Administrator in order to view the mounted system drive. Administrator account is not the same as an account with administrative privileges.
Then type:
Code:
explorer
This will run explorer as Administrator and will allow you to browse the mounted system partition.
 
Part 3 - Roll You Own UEFI Install
 
Let's start by downloading the needed files. Here is a customized UEFI boot for 32 &amp; 64 bit machines. https://www.androidfilehost.com/?w=files&flid=143191
 
**NOTE: If you came from our nougat builds to our Bliss-x86 8.x builds, you will have to edit the grub.cfga bit. Please see below **
If you are using Bliss-x86 8.x/10.x, please use the grub entry below as a guide:
Code:
menuentry 'Bliss-x86' --class android {
    search --file --no-floppy --set=root /AndroidOS/system.sfs
    linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS androidboot.selinux=permissive androidboot.hardware=android_x86_64 quiet DATA=
    initrd /AndroidOS/initrd.img
}
(EXT3/EXT4 installs) (NOTE: Due to a bug on ext3/ext4 installs, please use the grub setup below)
Code:
menuentry 'Bliss-x86' --class android {
    search --file --no-floppy --set=root /AndroidOS/system.sfs
    linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS  androidboot.selinux=permissive androidboot.hardware=android_x86_64 quiet DATA=
    initrd /AndroidOS/initrd.img
}
Now that we have the partition mounted, we can copy that BOOT dir to your UEFI partition using Explorer as admin or using the New Task dialog from Task Manager. (look up for a refresher on both of those) Once it is copied, go back to the admin CMD prompt and type:
 
Code:
mountvol X: /D
or if you used Z:, type:
 
Code:
mountvol Z: /D
This will dismount the UEFI/ESP volume for safe reboot. we then suggest you use EasyUEFI here to create the UEFI boot entry. Open the app, and create a new entry. Select your UEFI partition, and in the File Path, click Browse and use the file manager window to browse to your BOOT/grub/grubx64.efi file. Click OK, and then choose the new grub entry and move it to the top. Make sure secure boot is turned off or else it likely will just boot back to Windows.
 
 
Part 4 - The Manual Blissification of Your PC
 
To do a manual "Wubi like" install of Bliss-x86 after you install the UEFI entry, you will need to open the Bliss-x86 .iso/img with 7zip, and then drag all the .img & .sfs files to C:/android-x86 or whatever your target drive is (make sure your grub entries match where you are putting these). Then create your data.img, we suggest using a tool like one from XDA called RMXtools (use ver 1.7) to create it. Check the tool's thread for how to use it, but when you figure it out, you will want to create your data.img inside that android-x86 folder.
 
You can now reboot, if you have installed the custom UEFI entry right and selected it using EasyUEFI, you should boot right to the Android-x86 grub theme. There you can use up and down to select, and return to boot that entry. You can also hit e to edit the selected entry. You will want to pay attention to which entry you select, since there will be one for Bliss-x86(32bit) and one or Bliss-x86_64(64bit).
 
 
 
Install Bliss OS using a VM (virtualbox)
 
This method does require some beefy PC specs, so it might not work for all. (Info provided by Chih-Wei Huang, from Android-x86 Project)
We could fill up an entire section on this part alone, so here's a couple videos to help you figure things out.
 
If things still aren't working right for you, chances are it's hardware related. 
( check cat /proc/cpuinfo ) 
 
 
 
Specific Surface model instructions
 
From posts: 
https://forum.xda-developers.com/showpost.php?p=78015375
https://forum.xda-developers.com/showpost.php?p=76896155
 
Many of you have a Surface device and would like to run Android on it, and you're in luck, because this project was started with a Surface Pro 3 to test with back in Android Nougat. I have since retired that device and have been developing with a Surface Book, an IPTS device. This means it requires a specific set of firmware for this device to work right. For all other Surface models, it will require the user to upgrade their own firmware, because it is too large of a hassle for me to build for all Surface models without having those devices to test with. So I present to you a simple guide to get what you need quickly and perform the updates yourself. 
 
To start, you will need to grab the right firmware for your device:
 
Surface Series Devices:
 
Series 5 devices: Surface Book 2, Surface Pro 2017
Series 4 devices: Surface Book, Surface Pro 4, Surface Laptop
Series 3 devices: Surface Pro 3
For the ipts_firmware files (series 4/5 devices only), please select the version for your device.
 
v76 for the Surface Book
v78 for the Surface Pro 4
v79 for the Surface Laptop
v101 for Surface Book 2 15"
v102 for the Surface Pro 2017
v137 for the Surface Book 2 13"
For the i915_firmware files (series 3/4/5 devices), please select the version for your device.
 
kbl for series 5 devices
skl for series 4 devices
bxt for series 3 devices
 
Firmware files can be found here:
https://github.com/jakeday/linux-sur...aster/firmware
 
For Surface Go users, you will have to remove some files and replace with the Jakeday's firmware. Please see this thread on Reddit: https://www.reddit.com/r/SurfaceLinu...o_ubuntu_1810/ 
 
Once you have the right firmware you need, you should copy it to a folder on your Bliss install (sdcard/surface), making sure to put the right firmware in the right folders:
IPTS firmware: sdcard/surface/intel/ipts
ath10k firmware (some models): sdcard/surface/ath10k
mrvl firmware (some models): sdcard/surface/mrvl
mwlwifi firmware (some models): sdcard/surface/mwlwifi
nvidia firmware for SB2: sdcard/surface/nvidia
 
Next, open a terminal (we include one you can enable in developer options). In the Terminal, enter the following commands, giving permission to the SU popup dialog when prompted: 
Code:
$ su
$ mv -f sdcard/surface/* system/lib/firmware/
Then you can restart:
Code:
$ reboot
It should recognize and load the correct firmware versions for your device upon reboot if you did everything correctly. 
 
 
 
Setting up Taskbar on Bliss OS - Pie 
 
(Enabling Taskbar as your navbar)
If you would like to use Taskbar as your default launcher, you will need to first go into Settings > Blissify > Gestures, and enable something like Carbon Gestures (I setup three-finger swipes, Right for Back, Down for Home, & Up for Recents), then you can go to Blissify > Buttons, and first, switch the navigation mode to SmartBar, then go back and disable the Navbar from there by switching off Allow Navigation Bar on the top. At this point, you need to switch to your homescreen, so swipe up with your gesture, or tap the Win key. Or Win+esc. Then launch Taskbar, enable it, set to launch on Boot, and I disable hiding, and enable a few other settings in Freeform and Advanced screens. Setting it up this way will prevent any crashing from happening on initial launch. And it allows you to also use Quickstep launcher as the main background. Again, here's a video showing how I use it currently 
 
https://youtu.be/htFC8poBEPY
 
 
 
Switching to Gapps - Thanks @wrwolf2 
 
https://forum.xda-developers.com/showpost.php?p=79289406
 
 
Watching Netflix - FOSS & GMS Builds 
 
Netflix from their support site, or Play Store tends to see our rooted OS as an incompatible device. In my tests, this version of Netflix seems to work great, as long as you don't update it (Select Cancel when the popup asks to update).
Download from ApkMirror: https://www.apkmirror.com/apk/netfli...15145-release/
 
 
!!Troubleshooting!! - First Steps 
 
Grub2 kernel parameters and options 
*** You will want to pay attention here *** 
 
 
With Bliss OS on the PC, we tend to use quite a few command line options to get things working right. we've gathered a few of them here to explain them a little bit. 
A full list of available kernel parameters can be found here: https://www.kernel.org/doc/Documenta...parameters.txt 
 
Options Overview:
parameter	Description
root=	Root filesystem.
rootflags=	Root filesystem mount options.
initrd=	Specify the location of the initial ramdisk.
init=	Run specified binary instead of /sbin/init (symlinked to systemd in Arch) as init process.
init=/bin/sh	Boot to shell.
systemd.unit=	Boot to a specified target.
nomodeset	Disable Kernel mode setting.
panic=	Time before automatic reboot on kernel panic.
debug	Enable kernel debugging (events log level).
mem=	Force usage of a specific amount of memory to be used.
maxcpus=	Maximum number of processors that an SMP kernel will bring up during bootup.
selinux=	Disable or enable SELinux at boot time.
netdev=	Network devices parameters.
video=<videosetting>	Override framebuffer video defaults.
 
sleep=1 
This will enable the system.prop value for sleep.earlysuspend=1, and on some machines, it enables the proper sleep state. 
 
acpi_sleep=s3_bios,s3_mode
Sometimes needed for older machines to enter sleep mode properly
 
SETUPWIZARD=0
This command will skip SetupWizard on boot. (Only needs to be run once)
 
AUTO_LOAD=old
This will load android-x86 variants using the old modprobe method to init devices. We sometimes use this to debug devices not starting. 
 
DEBUG=1 & DEBUG=2
These enable verbose console debugging, giving another command shell after loading kernel modules, but before Android init
 
vga=xxx & video=
These are the common video modes that you can boot into if it doesn't pick the best choice automagically 
You can also use video= as resolution parameters: video=LVDS-1:d video=1366x800 , learn more from our own 
Henri Koivuneva: https://groups.google.com/forum/#!msg/android-x86/jSF3RnADnqA/1sfYdGV_AQAJ
 
nomodeset 
This will load mostly everything in software rendering/support mode. No hardware acceleration. Good for debugging. 
 
HWACCELL=1
This will disable graphics hardware acceleration, enabling rendering through Swiftshader. (Must use this if running headless)
 
buildvariant=eng, user, userdebug
This is the command line perimeter to run the current build as eng, userdebug, or user 
 
DPI=xxx
This will manually set the DPI on init. Use this if things are too big/small for you. 
 
fbcon=variablename
This is to configure framebuffer to use various options. Usually used to help fix video settings, etc. Even default rotation on some Atom tablets. Example:
Code:
video=efifb fbcon=rotate:1
As an example, here are a few of the boot options I use in testing:
Code:
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
 
