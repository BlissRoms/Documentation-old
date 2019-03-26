
# Git Started

## Foreword

This guide helps people learn `git` commands and how to get the features you want into your favorite ROM and make your own builds. This will help new people get started and will be a great refresher for veteran `git` users and has instructions for Arch Linux users as well. The document will be ever-changing and evolving because even we will be learning new material and will be putting it in here.

### Installation

In order to use `git review` we need it installed on our local machine.  For Ubuntu we use the package manager to install it.

    sudo apt-get install git-review

Or for Arch based installs, use:

    sudo pacman -S git-review

### Get the terminal set up

First, let's make sure our local `git` and Gerrit have the same email address associated.

    cd
    cat .gitconfig

This command should show something along those lines:

    [user]
    email = you@youremail.com
    name = Your Name

Edit any incorrect details. Your email and name is used to sign off commits, various changes, etc, so it is important that they are the same ones found on Gerrit and GitHub.

Next we will create a SSH key named “gerrit” to communicate with the Gerrit server.

    ssh-keygen -t rsa -C you@youremail.com -f ~/.ssh/gerrit

Again, note the email address must be the same here, your `.gitconfig`, and on Gerrit.

### Register at Gerrit

Open your favorite browser and go to [http://review.blissroms.com](http://review.blissroms.com). You will see a “Sign In” button on the top right - click that and register with GitHub.  Please note that you will need to use the same email address that you registered with GitHub for your terminal interactions with Gerrit.

Now we need to add our SSH key to Gerrit. Your name will be on the top right of the page.  Click your name and you should see “Settings”.  Once there click on the link “SSH Public Keys” on the left sidebar. Once it loads, click "Add Key" on the right.

Let's copy the key from the terminal and paste it into the browser.

    cat ~/.ssh/gerrit.pub

This should show something like:

    ssh-rsa AAAABBBBBBBBBBCCCCCCCCCCCCCCCCCCDjhl8768usdfsdfuhf/BlahBlahBlah/ThIs+1s/A/fAk3+k3Y/Hdhs+PlCesTvQqfaqHTHwdzGhn2lKVt14WYvQEDKVM9JoE9e8xarNWYa0ZspB1MGn2RVJ3xRp0+Q/pA237uBCl62yTbVGtKQBZB6Q+7A54z795U+G2wCb1rAQnI5yn5q/pQ4NhB0BLml/QRmjn/S8PldEge9Hfdh4Ifdk4r9DKSiicf7IK56jklDHKkJDFkjh/3345L10sTyre3JOeZyvr5SJdyqMtmMv+uSGF28fgZ6/OEO/yBY/eYEI/XVRDaRRat8nGHGae0T4dx me@myemail.com

Starting with the line beginning with ssh-rsa and ending with the email, paste that bit into the browser and click “Add”.

Then, add the new SSH key to your local identity in terminal.

    eval `ssh-agent`

This should print off a bunch of lines. We’re doing this to launch the ssh-agent before adding the key.

    ssh-add ~/.ssh/gerrit

This will result in something along the lines of:

    Identity added: /home/yourusername/.ssh/gerrit (/home/yourusername/.ssh/gerrit)

If you don’t get this, and get this instead:

    Could not open a connection to your authentication agent.

Or this:

    Error connecting to agent: No such file or directory.

This is because in recent versions of Ubuntu, you need to change how you launch `ssh-agent`. Run the command below, then try adding your SSH key again.

    eval “$(ssh-agent)”

Now we should be ready to use `git review`.

### Reviewing Code

Just `cd` to your source code directory and find the package you want to look at from Gerrit.  I’ll use `frameworks/base` as an example.

    # cd into source code directory
    cd ~/bliss/p9.0

    # Find project to work on
    cd frameworks/base

Let’s setup the remote to Gerrit:

    git review -s

This should create a `gerrit` remote.  If you added a passphrase to your SSH key you will have to enter that.

However, if this command outputs something like this:

    Could not connect to gerrit.
    Enter your gerrit username: 

And after you input your username, it shows something like this:

    Trying again with ssh://username@review.blissroms.com:29418/platform_frameworks_base.git
    <traceback object at 0x7f7f95ea9e18>
    We don't know where your gerrit is. Please manually create a remote named "gerrit" and try again.
    Could not connect to gerrit at ssh://username@review.blissroms.com:29418/platform_frameworks_base.git
    Traceback (most recent call last):
      File "/usr/bin/git-review", line 10, in <module>
        sys.exit(main())
      File "/usr/lib/python2.7/dist-packages/git_review/cmd.py", line 1534, in main
        sys.exit(e.EXIT_CODE)
    AttributeError: 'GitReviewException' object has no attribute 'EXIT_CODE'

Why does this happen? This is because the `ssh-agent` process got killed after prolonged use of the system, so you need to restart it.

As a quick and dirty fix, run this again:

    eval “$(ssh-agent)”

This should fix it. If not, then you need to go back to Chapter 1 and add your existing key. Don’t make a new key, then you’d need to add a new public key to the Bliss Gerrit.

Now let’s see if there are any changes we can review.

    git review -l

This outputs:

    2433  new-mm6.0  SystemUI: fix double tap power launching custom lockscreen icon
    2432  new-mm6.0  SystemUI: remove extraneous debug log
    Found 2 items for review

#### My output is blank (or different.) Why?

This is because right now, there are no changes to be reviewed in `frameworks/base` (or there are different commits to be reviewed.) This is completely normal and to be expected, as we merge and submit new commits frequently. The guide will probably be out of sync with current changes by the time you are reading this.

Now you have a list of commits sorted by change number with a description of what changes were made.

We have a few choices of how to pick commits.

#### Cherry-picking commits to current branch

    git review -x changenumber

will cherry-pick that commit to the current branch you are in.  A benefit of this is the next time you `repo sync –force-sync`, it will discard the commits you picked.

#### Cherry-picking commits to new branch

    git review -d changenumber

will cherry-pick to a new branch based on the submitter’s name and change ID.  If you choose this route the change will be in its own branch and when you are done you can just delete that branch.

    Switched to a new branch review/yourusername/changenumber

Now you can review the change. Follow these guidelines:

- Did the ROM build with the change incorporated?
- Did the added feature or fix work in the new build?
- Did the new commit break any other aspects of the build?

After reviewing these guidelines, log into Gerrit via your web browser, and click on the commit you reviewed. Select the ‘Reply’ button on the top and choose your score. +1 if it works as intended, -1 if not. Be sure to leave comments on why you gave your score (such as “commit A does not show in 320 DPI”, etc)

Once you're done, return to the base directory (`croot`) to follow along with the next example!

### Submitting code

Go to the directory of the package or app you want to make changes to. We will use `frameworks/base` as an example.

    cd frameworks/base

Setup the remote to Gerrit if you haven’t already:

    git review -s

Next, make sure you are on the correct branch:

    git branch -t p9.0 BlissRoms/p9.0

This makes sure that your local branch follows the remote branch. `-t` stands for tracking. Now we need to `checkout`, to switch to that new branch we tracked. You usually do not need to do the above command, but it is a nice check to do if something is going south.

    git checkout p9.0

This should output:

    Changed to new branch p9.0 tracking upstream BlissRoms/p9.0

Now make your changes, cherry-picks, etc. and commit them. If you need a guide to do that, check out [Picking from other sources](#picking-from-other-sources). When you’re done, come back here! Then all you have to do to send it to gerrit is:

    git review

If you are submitting multiple commits it will ask if you really want to, just type “yes” at the prompt.

### Picking from other sources

So you’ve successfully learned how to commit and send it to Bliss Gerrit. But you need to find something to commit. Thankfully, there are lots of examples you can try out. You can check other teams’ Gerrits and cherry-pick from them.

For this demonstration, I’m going to use LineageOS's Gerrit. Find the commit you want, and note down the commit ID.

Now, we need to add LOS’s repositories. If you’re pulling over a commit, you must make sure that we track the repository by going to Bliss’s Gerrit and checking in Projects > List. If the repository is not there, it usually means we aren’t tracking it.

    git remote add los https://github.com/LineageOS/……(your repo name)
    git remote fetch los lineage-16.0

We’ve finished adding LOS’s repositories. Now you need to grab the commit ID you copied earlier and paste it on the command underneath.

    git cherry-pick 9ff831

Replace 9ff831 with your own commit ID. If `git` complains about duplicate revisions, paste the full SHA1 hash instead of the shorthand `git` uses.

Before pushing and submitting, we need to check if our commit was successfully committed. Type the bottom command:

    git log --oneline

After you check that your commit is there, you can push `q` to quit. Then you can follow the instructions from [Submitting code](#submitting-code) again.

### Conclusion

Don’t worry if you don’t get it at once. It’s a fairly long process of submitting and reviewing at Gerrit. Read the guide again and again and you’ll start to remember commands without looking at this. I will continue with this as I learn more things, I hope this has helped someone.

-- Vaughn (rwaterspf1)

-- Eric Park (ideaman924)

-- Jon West (electrikjesus)
