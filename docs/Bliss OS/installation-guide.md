
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

## Windows installation method

**This is the current recommended method for beginners!**

We recommend beginners to use this method as it is the least error-prone and non-destructive. The following instructions were adapted from the Android-x86 project, so some of the images are from them. [To look at Android-x86's original installation guide, click here.](https://www.android-x86.org/installhowto.html)

When booting into the installer, choose "Installation - Install Android-x86 to harddisk":

![beginner-installation-install-1](https://i.imgur.com/gEnnGFp.png)

Once the installer boots, you will be asked to select the target drive. Choose the NTFS drive that houses your current Windows install. You do **not** need a separate partition, as the installer will create an image on your Windows partition.

![beginner-installation-install-2](https://i.imgur.com/fpMo5GS.png)

Choose "Do not re-format" on the next screen. It is important that you choose "Do not re-format", as any other option will cause the installer to continue with the ["Bootable installation method"](#bootable-installation-method-mbruefiesp-3264-bit), which **will** result in **permanent data loss**, including your Windows partition!

![beginner-installation-install-3](https://i.imgur.com/QSDt8ia.png)

Choose "Yes" when prompted about the `grub` bootloader:

![beginner-installation-install-4](https://i.imgur.com/PfmjHHi.png)

The installer will ask whether or not you want to make the system partition read/write-able. If you want to root your installation, you will choose "Yes" here. Otherwise, choose "No."

![beginner-installation-install-5](https://i.imgur.com/SXEeevy.png)

The installer will begin to write the changes to the disk. This will take some time. Go grab a coffee!

![beginner-installation-install-6](https://i.imgur.com/iQ4tEAD.png)

Then the installer will ask you how much space you want to allocate for the data image. Most users choose 8 GB, 16 GB, or 32 GB.

Congratulations! You should now have a functional dual-boot with Bliss OS!

## Bootable installation method - MBR/UEFI/ESP (32/64-bit)

**This is the current recommended method for advanced users!**
 
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


## Conclusion

Thanks for following along! If you want to check out the next guide, [click here!](extras.md)

Having problems with your new installation? [Check out Troubleshooting.](troubleshooting.md)
