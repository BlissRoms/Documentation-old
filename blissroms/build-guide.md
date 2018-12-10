BuildGuide v2
Updated for Nougat (n7.1)

Introduction
	Since a lot of custom ROMs feature wikis to build devices by users themselves, I figured why not. This is the official guide to build Bliss for your own device.
	The golden rule to building is patience. If something breaks, wait for your maintainer to fix it, send a polite message to your maintainer, or better yet, try and fix it yourself. Then you can make a merge request and contribute!
	Let’s get started.

1. Preparation
	I’m assuming your dual-boot or whatever setup you have of Linux is running, and you have a console window open in front of you. Good.
	You need at least 200GB of drive space. 100GB may also be fine, I just like 200GB as a cutline because there’s a safe wiggle room. Also, if you’re going to use ccache, it’s best if you don’t argue and stick to the 200GB free space rule.
	Finally, for speed - I recommend SSDs and good CPUs, but you can run builds on crappy hardware. Just be aware that it might take more than a few hours if you do.
	For RAM, you need at LEAST 8GB. If you have less then it will choke the build system and it will fail. Always. It’s guaranteed. More RAM = more speed, so please don’t use less amounts of RAM!

a. Install the JDK
	First, you need to install Java. This section used to be long and tedious, but thankfully Google ditched Oracle and went with the open-sourced OpenJDK.
	$ sudo apt install openjdk-8-jdk

b. Install build tools
	This is probably a bit disconcerting… you can’t memorize this step. Probably.
	If using Ubuntu 14.x, just copy the below command and carry on.
	$ sudo apt install bison build-essential curl flex git gnupg gperf libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk2.8-dev libxml2 libxml2-utils lzop maven pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev g++-multilib gcc-multilib lib32ncurses5-dev lib32readline-gplv2-dev lib32z1-dev git-core libc6-dev-i386 x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev unzip
You on Ubuntu 15.x? Use this command. Don’t run the above command.
	$ sudo apt install bison build-essential curl flex git gnupg gperf libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk2.8-dev libxml2 libxml2-utils lzop maven pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev g++-multilib gcc-multilib lib32ncurses5-dev lib32readline6-dev lib32z1-dev git-core libc6-dev-i386 x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev unzip
	You on Ubuntu 16.x or newer? Use this command. Don’t run the above commands.
	$ sudo apt install bison build-essential curl flex git gnupg gperf libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk3.0-dev libxml2 libxml2-utils lzop maven pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev g++-multilib gcc-multilib lib32ncurses5-dev lib32readline6-dev lib32z1-dev git-core libc6-dev-i386 x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev unzip
	Whew! We’ve done the hard part of the entire getting-prepared stuff. So now let’s move on to easier tasks. We only need to download the hefty source code.

c. Download tools for source code
	Now we need to get the source code.. via a handy program named ‘repo’. Google made this initially to deal with increasing repositories needed to build Android. This scours all the repositories needed and downloads the whopping >20GB source code you need to build Android.
	So open up a Terminal and use this command:
	$ mkdir ~/bin
	Now we need to download the ‘repo’ tool. Use this:
	$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
	This command downloads the ‘repo’ program and places it in the ~/bin. Now we need to make it executable. It’s no good to us if we can’t execute a program, can we? Use this command:
	$ chmod a+x ~/bin/repo
	Okay! And we need to add ~/bin to the .bashrc:
	$ nano .bashrc
	Scroll to the end of the file and type this line:
	$ # Export ~/bin
	$ export PATH=~/bin:$PATH
	Again, Ctrl-O and enter to save, then Ctrl-X to exit nano. Remember to source the file or relaunch Terminal. If you forgot how to source your .bashrc (sourcing just means that it asks Terminal to read the changes you made to its configuration), then just use the below command:
	$ source .bashrc
	Which can be shortened to:
	$ . .bashrc

c-1. Alternatives!
	If you need the repo tool to be available anywhere (say you’re setting up Jenkins or shit like that) you will need to first download repo to your home directory, move it with sudo and give it executable permissions. The steps will be like:
	$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/repo
	$ sudo mv ~/repo /usr/bin/
	$ chmod a+x /usr/bin/repo
	Repo will now work, without any .bashrc modifications. However, these steps aren’t recommended, since repo hasn’t been audited yet (and probably never will because it doesn’t deal with sensitive information.) Be careful, and if your system gets compromised we take no responsibilities.

Now we’re ready to download the source code.

d. Download!
	And now we come to the big moment. We need to make a folder where your source code can reside. Run the below command:
	$ mkdir ~/Bliss/system
	Then, run this:
	$ cd ~/Bliss/system
	Now, we’re in the Bliss folder. This is where all the source code is going to be downloaded.
	But! Before we download, we need to tell ‘repo’ and ‘git’ who we are. Run the below commands, substituting your information:
	$ git config --global user.email “you@example.com”
	$ git config --global user.name “Your Name”
	Now, we’re ready to initialize. We need to tell ‘repo’ which source code we’re about to download, which is Bliss. Type this:
	$ repo init -u https://github.com/BlissRoms/platform_manifest.git -b n7.1
	-b is for the branch, and we’re on n7.1, Nougat. It’ll take a couple of seconds. You may need to type ‘y’ for the color prompt.
	Then, after that is done, you have two options. One is to just download and let repo run wild:
	$ repo sync
	Two is to fine tune repo to your computer.
	$ repo sync -j# -c
	-j is for threads. Typically, your CPU core count is your thread count, unless you’re using an Intel CPU with hyperthreading. In that case, the thread count is double the count of your CPU cores. So in my example, it would be like:
	$ repo sync -j4 -c
	-c is for pulling in only the current branch, instead of the entire history. This is useful if you need the downloads fast and don’t want the entire history to be downloaded.
	By default, repo will use 4 threads and use the -c prefix. So the above was a trick question. Plain old repo sync will use the above configuration.
	Repo will start downloading all the code. That’s going to be slow, even on a fiber network. While waiting, grab a cup of coffee and congratulate yourself. You’re on your way!

2. Build it!
	Before we build though, we need to ask the build system to use Java 8. Currently, Java 8 isn’t officially supported, and there’s a flag we need to set to make it ‘experimentally’ compatible.
	Run this again:
	$ nano .bashrc
	Again, Ctrl-O and enter to save and Ctrl-X to exit. Remember to source the file or relaunch Terminal.
	In this guide, we will only cover official devices with actual maintainers. First, we need to ask the bash instance to learn some new building tricks. Execute this:
	$ . build/envsetup.sh
	What did that do? Doesn’t it seem familiar? . is for source. But what does source do? It asks the bash instance to read the files to grab new changes. But what does build/envsetup.sh have?
	Simple. It has all the commands you need to execute a build. You need to do this every time you’re about to run a build, or your commands will error out.
	Now, we need to tell the build system what device you’re going to build. I’m going to build for my device, the Nexus 5X, as an example. We need the codename of the device. You can search up the codename on Google. In my case, my device’s codename is bullhead.
	Execute this:
	$ breakfast (codename)
	So it would be this for me:
	$ breakfast bullhead
	What does this do? Breakfast searches repositories for your device ‘tree’, which contains all the details needed to make the build your own. CPU, kernel info, device screen size, whether the board has Bluetooth, NFC, what frequencies the build needs for Wi-Fi…that sort of stuff. Breakfast will automatically search in the BlissRoms-Devices github repository, and grab your device tree for you.
	Okay, so we have the device tree set up. A fun trick:
	$ croot
	Will dump you back in the root of the source code tree. So if you’ve been messing around with folders and don’t have any idea where you are, you can use the above command. This command, however, needs you to have sourced the build/envsetup.sh script earlier.
	So we’re ready to build! Two options:
	One is to use:
	$ brunch (codename)
	So:
	$ brunch bullhead
	But what is brunch? It is a compact form of these commands:
	$ breakfast bullhead
	$ mka bacon
	Sounding complicated? Brunch will automatically pull the device  trees again or check if it’s there by running breakfast. Then it’ll call upon the build system to build a full zip for your device.
	The second option is to just use this:
	$mka bacon
	Because we’ve already done breakfast earlier. Personally, I like brunch, however.
	The building process would take a long time… and be prepared to wait a little longer than repo sync, depending on your hardware speed, etc.

3. After building?
	So now, the build completed and it says ‘make completed successfully’ in green letters. Excellent.
	Now, you need to grab the zip for your device. Type this command:
	$ cd out/target/product/(codename)/
	So this would be:
	$ cd out/target/product/bullhead/
	For me. In here, you’ll find a zip that goes along the lines of ‘Bliss-v6….” This is your zip file. This is your handcrafted ROM. Congratulations!

4. Troubleshooting
	If your build failed… Darn it!
	Well, there’s a few stuff you can try.
Try a fresh ‘repo sync’ to make your repository up to date
Make sure your dependencies are installed correctly
Make sure you sourced build/envsetup.sh
Make sure you’re at the root of the build tree
Make sure you ran breakfast correctly and it did not error out
Make sure your computer itself isn’t faulty 
	If nothing fixes the problem, you can ask us via Hangouts. Get to the G+ Build Support.

Conclusion
	Again, if something goes wrong, use the help support Hangouts or use Google. Most of the errors you encounter is due to misconfiguration and wrong commands entered.
	This guide is automatically saved to your Google account after you have read it. You need to go to docs.google.com to read it again. Building a ROM is very hard and tedious - so don’t hit yourself too hard for not remembering the steps. Remember, this guide will automatically update to handle new errors and use-cases.
	After you finish building, you can try out the Git Started guide above. Make changes, commit, and send them off to Bliss gerrit for review! Or better yet, get access to new changes that isn’t on the official channel for the moment. Again, ROM building is a fun project you can work with. I hope this guide was a lot of fun to run through!
	-ideaman924 aka Eric
