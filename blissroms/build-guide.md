# BuildGuide
## Updated for Pie (p9.0)

### Introduction

This is the official guide to build BlissRoms for your device. In this guide, we will only cover official devices with actual maintainers. We will not delve into porting devices.

The golden rule to building is patience. If something breaks, wait for your maintainer to fix it, send a polite message to your maintainer, or better yet, try and fix it yourself. Then you can make a merge request and contribute!

Let’s get started.

### Preparation

Prerequisites of this guide: Linux box, at least 200GB space of HDD, at least 8GB RAM. A decent CPU (or CPUs if you have a server motherboard) is recommended.

You may try building on crappy hardware but there is no guarantee your build will succeed or even function at all.

### Install the JDK

Install OpenJDK:

    sudo apt install openjdk-8-jdk

### Install build tools

For Ubuntu 14.x:

    sudo apt install bison build-essential curl flex git gnupg gperf libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk2.8-dev libxml2 libxml2-utils lzop maven pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev g++-multilib gcc-multilib lib32ncurses5-dev lib32readline-gplv2-dev lib32z1-dev git-core libc6-dev-i386 x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev unzip

For Ubuntu 15.x:

    sudo apt install bison build-essential curl flex git gnupg gperf libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk2.8-dev libxml2 libxml2-utils lzop maven pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev g++-multilib gcc-multilib lib32ncurses5-dev lib32readline6-dev lib32z1-dev git-core libc6-dev-i386 x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev unzip

For Ubuntu 16.x (or newer):

    sudo apt install bison build-essential curl flex git gnupg gperf libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk3.0-dev libxml2 libxml2-utils lzop maven pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev g++-multilib gcc-multilib lib32ncurses5-dev lib32readline6-dev lib32z1-dev git-core libc6-dev-i386 x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev unzip

### Install source code tools

Now we need to get the source code via a program named `repo`, made by Google. The primary function of `repo` is to read a manifest file located in Bliss's GitHub, and find what repositories you need to actually build Android.

Create a `~/bin` directory for `repo` to live in:

    mkdir -p ~/bin

The `-p` flag instructs `mkdir` to _only_ create the directory if it does not exist in the first place. Now download the `repo` tool into `~/bin`:

    curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

Make `repo` executable:

    chmod a+x ~/bin/repo

And add it to PATH:

    nano .bashrc

Scroll to the end of the file and type these lines:

    # Export ~/bin
    export PATH=~/bin:$PATH

Ctrl-O and enter to save, then Ctrl-X to exit nano. Now either logout and login again (or reboot), or `source` the file:

    source .bashrc

Which can be shortened to:

    . .bashrc

#### What if I need `repo` globally?

If you need the `repo` tool to be available anywhere, you will need to first download `repo` to your home directory, move it with `sudo` and give it executable permissions. The exact commands are as follows:

    curl https://storage.googleapis.com/git-repo-downloads/repo > ~/repo
    sudo mv ~/repo /usr/bin/
    chmod a+x /usr/bin/repo

`repo` will now work anywhere, without any `.bashrc` modifications. However, these steps aren’t recommended as `repo` might become a security risk if a vulnerability is found.

Now we’re ready to download the source code.

### Download

Create a directory for the source:

    mkdir -p ~/bliss/p9.0
    cd -p ~/bliss/p9.0

Before we download, we need to tell `repo` and `git` who we are. Run the following commands, substituting your information:

    git config --global user.email “randy.mcrandyface@hotmail.net”
    git config --global user.name “Randy McRandyface”

Now, we’re ready to initialize. We need to tell `repo` which manifest to read:

    repo init -u https://github.com/BlissRoms/platform_manifest.git -b p9.0

`-b` is for the branch, and we’re on `p9.0`, Android Pie. It’ll take a couple of seconds. You may need to type `y` for the color prompt.

Then sync the source:

    repo sync -j24 -c

`-j` is for threads. Typically, your CPU core count is your thread count, unless you’re using an older Intel CPU with hyperthreading. In that case, the thread count is double the count of your CPU cores. Newer CPUs have dropped hyperthreading unless you have the i9, so check how many threads you have. If you have four threads, you would run:

    repo sync -j4 -c

`-c` is for pulling in only the current branch, instead of the entire history. This is useful if you need the downloads fast and don’t want the entire history to be downloaded. This is used by default unless specified otherwise.

`repo` will start downloading all the code. That’s going to be slow, even on a fiber network. Expect downloads to take more than a couple hours.

### Build

Set up the build environment:

    . build/envsetup.sh

Breaking down the command, `.` is for `source`. But what does `source` do? It gets aliases, functions and commands from the file and loads it into `bash` so it can be used. But what does `build/envsetup.sh` have? It has all the commands you need to execute a build. You need to do this every time you’re about to run a build, or `bash` will not be able to understand the build commands.

Define what device you’re going to build. For example, the Nexus 5X, has a codename of `bullhead`. You can check your specific device's codename on GitHub or on Google. Execute:

    breakfast bullhead

What does this do? `breakfast` searches repositories for your device "tree", which contains all the details needed to make the build suitable for your device. CPU, kernel info, device screen size, whether the board has Bluetooth, NFC, what frequencies the build needs for Wi-Fi, and a bunch of other things. `breakfast` will automatically search in the `BlissRoms-Devices` GitHub repository, and grab your device tree for you.

Okay, so we have the device tree set up. Feel free to browse around the source code to see what changed. You should see folders added to `device/`, `kernel/` and `vendor/`. A shortcut:

    croot

will dump you back in the root of the source code tree. So if you’ve been going through folders and don’t have any idea where you are, you can use the above command. This command, however, requires you to have `source`d `build/envsetup.sh` earlier.

So we’re ready to build! Run

    brunch bullhead

But what is `brunch`? It is a compact form of these commands:

    breakfast bullhead
    mka bacon

`brunch` will automatically pull the device trees again or check if it’s there already by running `breakfast`. Then it’ll call upon the build system to build a full `.zip` for your device.

Therefore, if you have already run `breakfast`, you can just run:

    mka bacon

The build process will take a long time. Prepare to wait a few hours, even on a decent machine.

#### I get an error about no `bacon` targets to build against, what's wrong?

Sometimes there is a bug, or the ROM developers do not implement a `bacon` target to build against. In this case, you will need to find what name they use to initialize a full build of the ROM. Conventionally, it is supposed to be `bacon`, but some ROM developers change this name inadvertently, or actually have a bug that causes the build target to never be found. If you cannot locate the build target, ask the developers of the ROM. Alternatively, you can try poking around in `build/make/core/Makefile` to see what the build target name is.

### After building

There are two outcomes to a build - either it fails and you get a red error message from `make`, or it succeeds and you see the Bliss logo in ASCII. If you encounter the former, you need to go back and fix whatever it's complaining about. Typically, 90% of the time the problem will be in your device tree. For the other 10%, submit a bug report to the ROM developers. Be sure to include the full log of your build to help diagnose the problem, and your device tree.

If you face the latter, congratulations! You've successfully built BlissRoms for your device. Grab the artifacts for your device:

    cd out/target/product/bullhead/

In here, you’ll find a `.zip` that goes along the lines of `Bliss-v11.5-Stable-bullhead-UNOFFICIAL-20180213-0621.zip`. Install TWRP, flash the build to your device, and enjoy!

### Troubleshooting

If your build failed, there are a couple things you can try.

* Try a fresh `repo sync` to make your repository up to date. Sometimes, the Internet connection between you and GitHub can be flaky. In rare cases a commit merge might be ongoing, and you might've grabbed an incomplete merge. Mostly, this should fix the issue 70% of the time.
* Make sure your dependencies are installed correctly. Error messages help out a lot here! Often it will say `shared/linked library not found` or something along those lines.
* Make sure you sourced `build/envsetup.sh`. This is especially common and worth suspecting if none of the build commands like `breakfast` and `lunch` work. If you have `repo sync`ed do this again.
* Make sure you’re at the root of the build tree. Again, to quickly jump there, use `croot`.
* Make sure you ran `breakfast` correctly and it did not error out. Missing device files will prevent successful builds.
* Make sure your computer itself isn’t faulty. HDDs usually die first, followed by RAM. SSDs rarely die but failure is not unheard of.  In extremely rare cases, your CPU may have a defect. If you're unsure, run a stress test via a program like Prime95.

Conclusion
	Again, if something goes wrong, use the help support Hangouts or use Google. Most of the errors you encounter is due to misconfiguration and wrong commands entered.
	This guide is automatically saved to your Google account after you have read it. You need to go to docs.google.com to read it again. Building a ROM is very hard and tedious - so don’t hit yourself too hard for not remembering the steps. Remember, this guide will automatically update to handle new errors and use-cases.
	After you finish building, you can try out the Git Started guide above. Make changes, commit, and send them off to Bliss gerrit for review! Or better yet, get access to new changes that isn’t on the official channel for the moment. Again, ROM building is a fun project you can work with. I hope this guide was a lot of fun to run through!
	-ideaman924 aka Eric
