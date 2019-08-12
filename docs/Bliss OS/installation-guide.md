
# Installation Guide

## Preface

These instructions are based on the Android-x86 project's installation guide. We have not changed the installer, so the procedure of installation is similar. We also thank @bg260 for his contributions - this guide was adapted partially from his work.


## Warning!

Team Bliss **does NOT accept any responsibility**. Users must read and understand the instructions, as the installation modifies core system files and carries a significant risk. You **accept all responsibility**, including but not limited to data loss and other malfunctions by continuing beyond this point.
 
Any questions, install issues, bug reports, etc. **MUST be** accompanied with the following things:

 - Log
 - Device info
 - Build info (file name)
 - Installation method (exact steps used)
 - Any other relevant information REQUIRED to diagnose your issue as NOT user error

If the following information is not supplied, your inquiry will be **ignored.**

These instructions have changed quite a bit for Android Pie, so consider this section a **work in progress.** Thank you for your patience!


## Bootable installation method - MBR/UEFI/ESP (32/64-bit)

**This is the current recommended method!**
 
Overview of the steps:

 - Download the ISO file
 - Use Rufus or similar to burn to USB drive
 - Disable Secure Boot, Bitlocker, and any other boot security software such as Veracrypt
 - Boot into the USB drive.
 - Run Bliss OS in Live mode to test things out. If all is well, continue to next step
 - Boot into the USB drive, and choose Bliss OS Install

Let's get started!

### Part 1 - Gather Your Tools
 
**Please note that this method is not supported on all machines.**
 
Download [Rufus](https://rufus.ie) and the 32-bit `.iso` or 64-bit `.iso`/`.img` file of Bliss OS, depending on the architecture of the machine you are installing Bliss OS on.

You will need a decent speed USB drive (4 GB or larger is recommended).
 
### Part 2 - Flashing Bliss OS to the USB drive
 
Plug in your USB drive, and load up Rufus. Once loaded, click on the icon next to the ISO Image dropdown menu. Now browse to where you have your Bliss OS (32-bit) `.iso`, _or_ your Bliss OS (64-bit) `.iso`/`.img` file. Once chosen, the dropdown should switch to the correct image type, and fill the rest in for you. Once you are ready, click Start.

### Part 3 - Testing Bliss OS on your system

**This is very important!** If you, as a user, **do NOT** test the OS first to make sure it is compatible with your device, please **do NOT** expect us to help you if you happen to install it blindly and something goes wrong.

Reboot your machine, and enter the BIOS. Most motherboards have the default key as "F2". Change the boot order so that the USB is the first thing the device will boot to. Once the boot orders are changed, reboot. If everything goes well, you should see a `grub` boot screen. Select the "Live CD" option, and if your machine is compatible, you should then see a little bit of text, and then the Bliss OS boot animation. This will go on for a few minutes, but should eventually boot to Bliss OS. If the system never boots to Bliss OS, this is a bad sign that your system might not be compatible. If it does boot, and you would like to install it, continue to the next step.
For those wanting to use root, you will need to install the OS and be running of that install. Root will not function properly in Live Mode. 

#### Troubleshooting - Booting from the USB kicks me back to BIOS, or back to my Windows/macOS/Linux installation.

Your drive is incompatible or you have formatted it incorrectly. Try flashing the image again to the drive with Rufus. If that does not work, your device does not support booting from USB and you will have to try an alternate method.
 
### Part 3 (alternate) - Using Bliss OS from your USB drive
 
If you choose to use Bliss from the USB drive, the data you modify or create on the live install will be in an ephemeral state unless you create a `data.img` to store the data. You can create a `data.img` in the root directory of the USB drive (make sure you have a minimum of 4-5 GB free on the drive). We suggest using a tool like RMXtools from XDA to create it (version 1.7 is recommended). Check the tool's thread for detailed usage instructions. You will want to create your `data.img` inside the root directory of your USB drive, with all the other `.img` files. From there, just boot into live mode, setup your system the way you want, and the data should be persistant across reboots.
 
### Part 4 - Setting up and Installing Bliss OS on your HDD/SSD/SD card
 
Quick warning again, in case you missed it. Team Bliss is **NOT responsible**, directly or indirectly, for any damages caused by this guide. By continuing, you automatically agree to these terms.
 
This is where things start to get a little tricky, especially with how devices vary. Make sure you have a backup plan in case something goes wrong.

Start off by opening your favorite partition management software, such as Disk Management in Windows, and create a new partition, making it the size you want (suggested minimum is 8 GB). Just format it to NTFS for now, because it will be formatted by the installer later into the process anyway. Remember what drive you have created here as it's important later on. For Windows machines, it will typically be `sda4` or `sda5`. Also create another 300 MB FAT32 partition for the `grub` bootloader to install to. (This part might require a third-party partition manager as Windows Disk Management might not let it be that small.)

Boot up the Bliss OS USB, and select the "Installation" option in `grub`. (It is the second one down, usually.)

The installer will load, and you will have an option to choose the partition you created earlier. Pick it, and select `ext4`. **DO NOT** blindly choose the partition, as an incorrect flash can mess up your drive and cause serious data loss. **You do NOT want to get this step wrong.** If you are unsure, boot back into Windows/macOS/Linux and write it down.

When it asks if you want to install system as R/W, select "YES" if you want to use root (SuperSU), and "No" if you do not need root.

When it asks if you want to install `grub`, select "Grub for Legacy BIOS boot type", "Grub2 for UEFI boot type", or neither if you are already running a Linux system. If you chose to install `grub`, the installer will allow you to choose the partition to install `grub` to. Make sure you select the 300 MB partition you set up earlier for `grub`.

The process will install and create the data directory/image, so be patient. When finished, the installer will then ask if you want to run Android-x86. You can just reboot here. Make sure you remove the USB drive.
 
If we have followed all the directions correctly, you should be presented with a `grub` boot menu. You can choose your `bliss_android_x86` option (or `android-x86`), and it will boot into Bliss OS. If you want to customize your `grub` boot entry, search the web first. We use the same `grub` setup that the Android-x86 project uses, so their forums will contain just about all the info you will need. 

Congratulations! We hope you enjoy using Bliss OS.

## Windows-based installer - UEFI/ESP (64-bit)

This method is **no longer supported** due to too many people not understanding computer basics and breaking things. **Proceed at your own risk.** This method might be the easiest currently if you understand what you are doing.

For the overall instructions on using this method, please refer to the [tool's original thread](https://forum.xda-developers.com/android/software/winapp-android-x86-installer-uefi-t3222483). The tools have been updated by Team Bliss for easy installation on UEFI/ESP machines. The [builds we produce can be found here.](https://github.com/BlissRoms-x86/Androidx86-Installer-for-Windows/tree/master/bin) And the [source for those builds can be found here.](https://github.com/BlissRoms-x86/Androidx86-Installer-for-Windows) This tool should work on Remix OS as well, but this has not been tested yet.

### Part 1 - Using the Installer
 
The installer has been updated to accept the `.iso` files for our 8.x/10.x/11.x releases. Just follow the prompts the installer gives. Refer to the original thread for any questions, and please search before asking.

If you plan on using root, the process will require you to manually extract the system.img from within the system.sfs file. Then you must delete the system.sfs file after extracting. 

**Warning** - for Pie, you will need to add `androidboot.hardware=android_x86_64` to the grub entry in order to boot!
 
### Part 2 - Switching the UEFI/EFI boot entry
 
Option one is to use the EasyUEFI tool, then switch the UEFI/EFI entry it created to boot first. Close and reboot. Option two is to use your BIOS to select the added UEFI boot entry.

## Use syslinux EFI to run Bliss OS 7.x/10.x/11.x 

Thanks to @IcedCube for the original post! This method is **NOT recommended** as it is fairly bleeding-edge and experimental, but it should help booting on Chinese tablets that do not want to run `grub`.

Use a Linux installation for the following procedure.

### Part 1 - Grab the required tools

Install `unsquashfs` (part of `squashfs-tools`).

### Part 2 - Get Bliss OS

Grab the latest build of Bliss OS 7.x/10.x/11.x.

### Part 3 - Get the syslinux EFI bootstrap 

[Grab the `.zip` file from @IcedCube's original post](https://forum.xda-developers.com/showpost.php?p=74977694&postcount=1237), and extract it to the root of the USB drive. This will bootstrap syslinux EFI onto it.

Then, make a folder called `android`.

Now, open up the `.iso` in an archive program. Extract the following files form the root directory of the `.iso` image to the USB drive's `android` folder:

 - `initrd.img`
 - `ramdisk.img`
 - `kernel`

Extract `system.sfs` to a folder somewhere, such as `/tmp`.

Open a terminal and change directory (using `cd`) to `/tmp`. Run `ls` and confirm that `system.sfs` is shown in the file list. If there is no output, start over as the file is misplaced.

Run the following:

`unsquashfs ./system.sfs`

This will make a new directory called `squashfs_root`.

### Part 4 - Version specific

#### If you are using Bliss 7.x

Change directory to `squashfs_root` and run `ls`. There should only be one file - a `system.img` inside the directory. Copy the file to the USB's `android` folder.

###  If you are using Bliss 10.x

Change directory to `squashfs_root`. The structure is a complete Android root filesystem. To install Bliss OS, the files will need to be in a system image. The following steps will guide you through creating a 2 GB `system.img` file, formatting it, mounting it, and copying the contents of `squashfs_root` into the new disk image.

Execute:

    mkdir /mnt/tempMount
    truncate /tmp/system.img --size=2G
    mkfs.ext4 -m0 /tmp/system.img
    sudo mount -o loop /tmp/system.img /mnt/tempMount
    sudo cp -prv /tmp/squashfs_root/* /mnt/tempMount/
    sync
    sudo umount /mnt/tempMount

The `sync` command might take some time.

Now copy the `/tmp/system.img` file to your USB's Android folder.

### Part 5 - Creating the data image

First, find where your USB drive is mounted. It is usually in `/mnt` or `/media` (ex. `/media/USB`).

`cd` into the `android` folder.

We will create a 3 GB data image file. You can attempt to create a 4 GB image but FAT32 maxes out at 4 GB per file. If your system supports exFAT or NTFS, you may try and use it.


    truncate data.img --size=3G
    mkfs.ext4 -m0 data.img
    sync

This will be an completely empty `ext4` disk image, but will be enough to run Bliss.

Finally, check to ensure everything is in structured like so:

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

Need to add some kernel parameters? Open `syslinux.cfg` and add them before the `initrd=/android/initrd.img` statement.

Unmount the USB from your computer. Plug it into your device and use the BIOS to boot from your UEFI USB Drive, partition 1. If all goes well, you will get a black screen with small white text saying "Booting Android..." followed by loading files. You should get the Linux kernel text, then see the Bliss boot animation play after a couple minutes depending on your USB drive read/write speed.


## Custom Install - Bliss OS 8.x/10.x/11.x UEFI/ESP (64-bit)
 
Just as a reminder, Team Bliss is **NOT** responsible for any damage caused by this guide. By continuing, you automatically agree to these terms.
 
### Part 1 - Mounting Your UEFI/ESP Partition
 
You will want to make sure you can view hidden and system files in Explorer options. Once you do that, hit the start menu, and type in `cmd`. Once "Command Prompt" shows up, right click on it and choose "Open as administrator".

#### `cmd` is not showing up, what should I do?

Press the Windows key and the R key to bring up the "Run..." dialog. Type in `cmd`, and then press Ctrl-Shift-Enter. Press "Yes" on the UAC popup.

Run the following:

    mountvol X: /S

Then check to see if it is mounted already. Run "Task Manager" by either

 - Pressing Ctrl-Alt-Del and then clicking on "Task Manager", or
 - Pressing Ctrl-Shift-Esc

Click on "File", "Run new task", "Browse", "This computer", and SYSTEM (X or type in `X:` in the filepath bar. If you cannot access `X:`, then that could mean one of three things.

 - You have an ESP setup (follow the installation method below)
 - You have a legacy MBR setup
 - Your setup has a custom boot sequence

### Part 1 (alternate) - ESP setup

Windows 10 sometimes has an EFI partition already mounted under drive letter `Z:`, hidden. A very quick and easy way to access the ESP (EFI System Partition) in Windows 10 without using the command line is to start "Task Manager" (check above if you forgot the steps), and then click on "File", "Run new task", "Browse", "This computer", and SYSTEM (Z or type in `Z:` in the filepath bar).

Now go to `boot/grub/grub.cfg` and edit it accordingly with Notepad++ or another text editor. Save the file and your're ready to go!
 
 
### Part 1 (alternate) - Killing the `explorer.exe`

Run `cmd` as admin and enter the following command:

    taskkill /im explorer.exe /f

This will kill the `explorer.exe` process - don't be surprised if it shows a warning. This step is sometimes required, because by default `explorer.exe` is ran by the currently logged in user, and it has to be run by the "Administrator" in order to view the mounted system drive. **The "Administrator" account is not the same as an account with administrative privileges.**

    mountvol X: /s

This will mount the system partition that usually consists of UEFI related files. `X:` is the letter of the drive - you can use whatever letter you want, but it has to be free for assignment. Then type:

    explorer

This will run `explorer` as "Administrator" and will allow you to browse the mounted system partition.
 
The above may not work for all devices, as some handle UEFI differently.

## Part 2 - UEFI installation
 
Let's start by downloading the required files. [Here is a customized UEFI boot for 32/64-bit machines.](https://www.androidfilehost.com/?w=files&flid=143191)
 
Please note that if you came from our Nougat builds to our Bliss OS 8.x builds, you will have to edit the `grub.cfga`.

If you are using Bliss OS 8.x/10.x, please use the `grub` entry below as a guide:

    menuentry 'Bliss-x86' --class android {
        search --file --no-floppy --set=root /AndroidOS/system.sfs
        linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS androidboot.selinux=permissive androidboot.hardware=android_x86_64 quiet DATA=
        initrd /AndroidOS/initrd.img
    }

If you are installing on `ext3`/`ext4`, due to a bug in the install you will have to use the following `grub` entry setup:

    menuentry 'Bliss-x86' --class android {
        search --file --no-floppy --set=root /AndroidOS/system.sfs
        linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS  androidboot.selinux=permissive androidboot.hardware=android_x86_64 quiet DATA=
        initrd /AndroidOS/initrd.img
    }

Now that we have the partition mounted, we can copy that `BOOT` directory to your UEFI partition using `explorer` as the Administrator or by using the "New Task" dialog from Task Manager. (See above if you forgot the steps!) Once it is copied, go back to the Administrator `cmd` prompt and type:

    mountvol X: /D

or if you used `Z:`, type:

    mountvol Z: /D

This will dismount the UEFI/ESP volume for safe reboot. We then suggest you use EasyUEFI here to create the UEFI boot entry. Open the app, and create a new entry. Select your UEFI partition, and in the "File" Path, click "Browse" and use the file manager window to browse to your `BOOT/grub/grubx64.efi` file. Click OK, and then choose the new `grub` entry and move it to the top. Make sure Secure Boot is turned off or else it likely will just boot back to Windows.

### Part 4 - The Manual Blissification of Your PC
 
To do a manual "Wubi like" install of Bliss OS after you install the UEFI entry, you will need to open the Bliss OS `.iso`/`.img` with 7zip, and then drag all the `.img` & `.sfs` files to `C:/android-x86` or whatever your target drive is (make sure your `grub` entries match where you are putting these). Then create your `data.img`. We suggest using a tool like RMXtools (use version 1.7) from XDA to create it. Check the tool's thread for detailed instructions. You will want to create your `data.img` inside that `android-x86` folder.

You can now reboot if you have installed the custom UEFI entry right and selected it using EasyUEFI. You should boot right to the Android-x86 `grub` theme. There, you can use up and down to select, and return to boot that entry. You can also hit `e` to edit the selected entry. You will want to pay attention to which entry you select, since there will be one for `Bliss-x86(32bit)` and one or `Bliss-x86_64(64bit)`.


## Install Bliss OS on a VM (virtualbox)

This method does require some beefy PC specs, so it might not work for all. Thanks to Chih-Wei Huang from Android-x86 Project for the detailed information!

This method is currently being worked on. Check back later for more information!

Befor we proceed, if things don't work out, check `cat /proc/cpuinfo` to determine if your CPU is compatible with virtualization.


## Specific Surface model instructions

Reference posts [here](https://forum.xda-developers.com/showpost.php?p=78015375&postcount=76) and [here](https://forum.xda-developers.com/showpost.php?p=76896155&postcount=2107).
 
Many of you have a Surface device and would like to run Android on it. You're in luck, because this project was started with a Surface Pro 3 to test with back in Android Nougat. The device has since been retired and development has continued on a Surface Book, an IPTS device. This means it requires a specific set of firmware for proper functioning. For all other Surface models, the user must upgrade their own firmware, because it is too large of a burden to build for all Surface models without having those devices to test with.
 
To start, you will need to grab the right firmware for your device.
 
Surface Series Devices:

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
 
 - v76 for the Surface Book
 - v78 for the Surface Pro 4
 - v79 for the Surface Laptop
 - v101 for Surface Book 2 15"
 - v102 for the Surface Pro 2017
 - v137 for the Surface Book 2 13"

For the i915_firmware files (series 3/4/5 devices), please select the correct version for your device:
 
 - kbl for series 5 devices
 - skl for series 4 devices
 - bxt for series 3 devices
 
All [firmware files can be found here.](https://github.com/jakeday/linux-surface/tree/master/firmware)
 
For Surface Go users, you will have to remove some files and replace with @jakeday's firmware. Please [see this thread on Reddit](https://www.reddit.com/r/SurfaceLinux/comments/9t53gq/wifi_fixed_on_surface_go_ubuntu_1810/) for detailed information.

Once you have the right firmware you need, you should copy it to a folder on your Bliss install (`SD_card_root/surface`), making sure to put the right firmware in the right folders:

 - IPTS firmware: `SD_card_root/surface/intel/ipts`
 - ath10k firmware (some models): `SD_card_root/surface/ath10k`
 - mrvl firmware (some models): `SD_card_root/surface/mrvl`
 - mwlwifi firmware (some models): `SD_card_root/surface/mwlwifi`
 - nvidia firmware for SB2: `SD_card_root/surface/nvidia`
 
Next, open a terminal (we include one you can enable in "Developer Options"). In the terminal, enter the following commands, giving permission to the superuser popup dialog when prompted:

    su
    mv -f SD_card_root/surface/* system/lib/firmware/

Then you can restart:

    reboot

It should recognize and load the correct firmware versions for your device upon reboot if you did everything correctly.

## Conclusion

Thanks for following along! If you want to check out the next guide, [click here!](extras.md)

Having problems with your new installation? [Check out Troubleshooting.](troubleshooting.md)
