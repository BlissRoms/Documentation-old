# Build Tips

Here's a collective list of things you can try to improve your builds. Have fun!

## `repo` optimization tips

### Threads

By default, `repo` only utilizes four threads (or whatever the ROM manifest declares.) If you have a stronger machine with more threads, you can increase this number. So, for example, if you have 24 threads, you would type:

    repo sync -j24

### Current history only

This should be set by default in your ROM manifest, but just in case, you can tell `repo` to only fetch recent changes. This allows for smaller downloads, which makes the sync quicker. Add the flag:

    repo sync -c

### Force

**Warning! Dangerous option. Read before proceeding!** Sometimes, your local history may go out of sync with remote repositories. This wouldn't be a problem if `repo` can handle it gracefully, but it doesn't and gives off a sync error. If you do not commit much, it may be worthwhile to add the force flags. Be warned though, **any changes you make to the repositories WILL BE DELETED.** Later down the line, if you commit a lot, you may want to remove this flag. To force sync, add the flag:

    repo sync -f --force-sync

### Minimal fetch

To disable syncing clone bundles and tags, use:

    repo sync --no-clone-bundle --no-tags

More documentation on this required, but for most developers these flags will be OK to use. This makes the sync faster as there is less information to sync over.

### Putting it all together

    repo sync -c -f -j24 --force-sync --no-clone-bundle --no-tags

That's quite long! How about we add this to our `.bashrc` as a alias? That way, we only have to type one phrase for `bash` to automatically type that out for us.

Open up `~/.bashrc` and add these lines:

    # Alias to sync
    alias reposync='repo sync -c -f -j24 --force-sync --no-clone-bundle --no-tags'

This way, next time you want to sync, just type `reposync` and `bash` will substitute the command for you. Easy! Just don't forget to `source ~/.bashrc` otherwise `bash` will not know of this new alias.

## Reset tree

**Warning! Very destructive tip. Do not use if you don't know what you are doing!**

While messing around with device specific folders, you may break something and the build process might not work. Or, you may have multiple devices synced and you want to delete it all and start over. However, you might have limited bandwidth, and might not want to download the source over again.

Add this function to your `~/.bashrc`:

    # Script to clean the source
    function resettree() {
        rm -rf device kernel vendor out .repo/local_manifests
        reposync
    }

Let's go over what this does, word by word:

 - `rm -rf`: Destructive command to erase all of the following:
 - `device` folder: Holds all device-related files
 - `kernel` folder: Holds all kernel-related files for devices
 - `vendor` folder: Holds all vendor-related files for devices AND ROM-specific vendor customizations
 - `out` folder: Stores artifacts of builds
 - `.repo/local_manifests` folder: Stores "manifest" information of devices to sync.
 - `reposync`: Executes the `repo sync` alias we made earlier.

The last line is important, because by deleting the `vendor` folder, we also delete some crucial files for building Bliss. To fix that, we rerun a sync. Note that because we did not delete any other folders, syncing and updating files only take a fraction of a time compared to starting from scratch.

To use this, after copying, don't forget to `source ~/.bashrc`. Then, run `resettree` at the base folder of the source code. Once you're done, don't forget to initialize a new device using `breakfast <devicename>`.

## GitHub cherry-pick

Thanks to @blueyes for providing the script!

Copy the following into your `~/.bashrc`:

    function gcp() {
        COMMIT=`echo "$1" | cut -d/ -f6`
        GH=`echo "$1" | cut -d/ -f1-3`
        if [ "$COMMIT" != "commit" ]; then
            echo -e "Please use a commit URL."
        elif [ "$GH" != "https://github.com" ]; then
            echo -e "Please use an https://github.com/ URL."
        else
            PROJECT=`echo "$1" | cut -d/ -f1-5`
            git fetch $PROJECT
            CP=`echo "$1" | cut -d/ -f7`
            git cherry-pick $CP
        fi
    }

To use this, `source ~/.bashrc` and then run `gcp <GitHub commit URL here>`.

## Make and go inside

This is a quick way to create and go inside a folder. Once again, copy this to your `~/.bashrc`:

    function mkcdir ()
    {
        mkdir -p -- "$1" &&
        cd -P -- "$1"
    }

Don't forget to `source`, and run `mkcdir <directory name>`.
