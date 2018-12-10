Git Started v4

Foreword
This guide helps people learn git commands and how to get the features you want into your favorite rom and make your own builds. This will help new people get started and will be a great refresher for veteran git users and has instructions for Arch Linux users as well. The document will be ever-changing and evolving because even we will be learning new material and will be putting it in here.

1. Installation
In order to use git review we need it installed on our local machine.  For Ubuntu we use the package manager to install it.
$ sudo apt-get install git-review
Or for Arch based installs, use 
$ sudo pacman -S git-review

1. Register at review.blissroms.com
Open your favorite browser and go to “http://review.blissroms.com ”.  you will see a “Sign in with Github” link on the top right, click that and register with github.  Please note that you will need to use the same email address that you registered with github for your terminal interactions with the gerrit.
2. Get the terminal set up.
First let's make sure our local git and the gerrit have the same email address associated with them.
$ cd
$ cat .gitconfig
This command should show something like:
[user]
email = you@youremail.com
name = Your Name
Edit any details if it’s incorrect. Your email and name is used to sign off commits, various changes, etc.
Next we will create a ssh key named “gerrit” to communicate with the gerrit server.
$ ssh-keygen -t rsa -C you@youremail.com -f ~/.ssh/gerrit
Again, note the email address must be the same here, your .gitconfig, and review.blissroms.com.
Now we need to add our ssh key to review.blissroms.com.  Open your browser and go to the website and login.  Your name will be on the top right of the page.  Click your name and you should see “Settings”.  Once there you will on the left “SSH Public Keys”, click that and on the right you will click “add key”.
In terminal, let’s copy the key and paste it into the browser.
$ cat ~/.ssh/gerrit.pub
This should show something like:
ssh-rsa AAAABBBBBBBBBBCCCCCCCCCCCCCCCCCCDjhl8768usdfsdfuhf/BlahBlahBlah/ThIs+1s/A/fAk3+k3Y/Hdhs+PlCesTvQqfaqHTHwdzGhn2lKVt14WYvQEDKVM9JoE9e8xarNWYa0ZspB1MGn2RVJ3xRp0+Q/pA237uBCl62yTbVGtKQBZB6Q+7A54z795U+G2wCb1rAQnI5yn5q/pQ4NhB0BLml/QRmjn/S8PldEge9Hfdh4Ifdk4r9DKSiicf7IK56jklDHKkJDFkjh/3345L10sTyre3JOeZyvr5SJdyqMtmMv+uSGF28fgZ6/OEO/yBY/eYEI/XVRDaRRat8nGHGae0T4dx me@myemail.com
Starting with the line beginning with ssh-rsa and ending with the email, paste that bit into the browser and click “Add”.
Add the new ssh key to your local identity in terminal.
$ eval `ssh-agent`
This should print off a bunch of lines. We’re doing this to launch the ssh-agent before adding the key.
$ ssh-add ~/.ssh/gerrit
This will result in something along the lines of:
Identity added: /home/yourusername/.ssh/gerrit (/home/yourusername/.ssh/gerrit)
If you don’t get this, and get this instead:
Could not open a connection to your authentication agent.
Or this:
Error connecting to agent: No such file or directory.
This is because in recent versions of Ubuntu, you need to change how you launch ssh-agent. Run the below code, then try adding your ssh key again.
	$ eval “$(ssh-agent)”
Now we should be ready to use git review.

2. Using git review on Bliss ROMs
1. Reviewing Code
Just cd to your source code directory and find the package you want to look at from gerrit.  I’ll use frameworks/base as the example.
$ cd ~/bliss/frameworks/base
Let’s setup the remote to gerrit.
$ git review -s
This should set a remote for ssh://you@review.blissroms.com.  If you added a passphrase to your ssh key you will have to enter that.
	However, if this command outputs something like this:
	Could not connect to gerrit.
Enter your gerrit username: 
	And after you input your username, it shows something like this:
Trying again with ssh://username@review.blissroms.com:29418/platform_device_qcom_sepolicy.git
<traceback object at 0x7f7f95ea9e18>
We don't know where your gerrit is. Please manually create a remote
named "gerrit" and try again.
Could not connect to gerrit at ssh://username@review.blissroms.com:29418/platform_device_qcom_sepolicy.git
Traceback (most recent call last):
  File "/usr/bin/git-review", line 10, in <module>
    sys.exit(main())
  File "/usr/lib/python2.7/dist-packages/git_review/cmd.py", line 1534, in main
    sys.exit(e.EXIT_CODE)
AttributeError: 'GitReviewException' object has no attribute 'EXIT_CODE'
Why does this happen? This is because the system didn’t load the SSH key you made earlier properly. This is because the ssh-agent process got killed after prolonged use of the system, and you didn’t restart it.
As a quick and dirty fix, run this again:
	$ eval “$(ssh-agent)”
	This should fix it. If not, then you need to go back to Chapter 1 and add your existing key. Don’t make a new key, then you’d need to add a new public key to the Bliss gerrit. Making multiple SSH keys increases chances of an attacker stealing your keys and making unauthorized, malicious changes.
Now let’s see if there any changes we can review.
$ git review -l 
2433  new-mm6.0  SystemUI: fix double tap power launching custom lockscreen icon
2432  new-mm6.0  SystemUI: remove extraneous debug log
Found 2 items for review
Now you have a list of commits sorted by change number with a description of what changes were made.  
We have a few choices of how to cherry-pick, 
$ git review -x changenumber
Will cherry-pick that commit to the current branch you are in.  A benefit of this is the next time you repo sync –force-sync it will discard the commits you picked.
$ git review -d changenumber
Will cherry-pick to a new branch based on the submitter’s name and change ID.  If you choose this route the change is in it’s own branch and when you are done you can just delete that branch.
“Switched to a new branch review/yourusername/changenumber”
Now you can review the change. Follow these guidelines:
Did the ROM build with the change incorporated?
Did the added feature or fix work in the new build?
Did the new commit break any other aspects of the build?
	After reviewing these guidelines, log into gerrit via your web browser, and click on the commit you reviewed. Select the ‘Reply’ button on the top and choose your score. +1 if it works as intended, -1 if not. Be sure to leave comments on why you gave your score (such as “commit A does not show in 320 DPI”, etc)

2. Submitting code

Go to the directory of the package or app you want to make changes to, again I’ll use frameworks/base as my example
$ cd ~/bliss/frameworks/base
Setup the remote to gerrit if you haven’t already:
$ git review -s
Next make sure you are on the correct branch:
$ git branch -t mm6.0 BlissRoms/mm6.0
This makes sure that your local branch follows the remote branch. -t stands for tracking. Now we need to checkout, to switch to that new branch we tracked. You usually do not need to do the above command, but it is a nice check to do if something is going wrong.
$ git checkout mm6.0
“Changed to new branch mm6.0 tracking upstream BlissRoms/mm6.0”
Now make your changes, cherry-picks, etc. and commit them. If you need a guide to do that, skip on over to Chapter 3. When you’re done, come back here! Then all you have to do to send it to gerrit is:
$ git review
If you are submitting multiple commits it will ask if you really want to, just type “yes” at the prompt.

3. Picking from other sources
	So you’ve successfully learned how to commit and send it to Bliss gerrit. But you need to find something to commit. Thankfully, there are lots of examples you can try out. You can check other teams’ gerrits and cherry-pick from them.
	For this demonstration, I’m going to use the Cyanogenmod gerrit. Find the commit you want, and note down the commit ID. You don’t need to write it down or anything, just copy it.
	Now, we need to add CM’s repositories. If you’re pulling over a commit, you must make sure that we track the repository by going to Bliss’s gerrit and checking in Projects > List. If the repository is not there, it usually means we aren’t tracking it.
	$ git remote add cm https://github.com/Cyanogenmod/……(your repo name)
	$ git remote fetch cm cm-13.0
	We’ve finished adding CM’s repositories. Now you need to grab the commit ID you copied earlier and paste it on the command underneath.
	$ git cherry-pick 9ff831
	Replace 9ff831 with your own commit ID. It’s usually longer than the example I showed you.
	Before pushing and submitting, we need to check if our commit made it up there, successfully. Type the bottom command:
	$ git log --oneline
	After you check that your commit is there, you can push q to quit. Then you can follow the instructions from Chapter 2 again.

Conclusion

If you have read this guide, this spreadsheet would be automatically saved to the logged in Google account. You need to go to spreadsheets.google.com and click on this guide again to read it.
Don’t worry if you don’t get it at once. It’s a fairly long process of submitting and reviewing at Gerrit. Read the guide again and again and you’ll start to remember commands without looking at this.
I will continue with this as I learn more things, I hope this has helped someone. 

-rwaterspf1 aka Vaughn
	-ideaman924 aka Eric Park
	-electrikjesus aka Jon West

