# Build Tips

Here's a collective list of things you can try to improve your builds. Have fun!

## `repo` optimization tips

### Threads

By default, `repo` only utilizes four threads (or whatever the ROM manifest declares.) If you have a stronger machine with more threads, you can increase this number. For example, to use 8 threads:

    repo sync -j8

To use all of the threads on your machine, use:

    repo sync -j$(nproc --all)

### Current branch only

This is usually set by default in your ROM manifest, but just in case, you can tell `repo` to only fetch the branch you want to work on:

    repo sync -c

### Current history only

To only download the most recent changes, use this flag:

    repo sync --depth=1

This will make `repo` fetch the most recent changes.

### Minimal fetch

To disable syncing clone bundles and tags, use:

    repo sync --no-clone-bundle --no-tags

`repo` uses `git` bundles over HTTP to download repositories. To disable this behavior, we use the `--no-clone-bundle` flag. We also don't need all of the `git` tags in each repository, so we disable that too with `--no-tags`.

### Force sync

Sometimes, bad Internet conditions may cause your `.repo` directory to become corrupt and not sync. `repo` keeps two copies of a repository - one under `.repo`, and one in the root of your source directory. If the repository in `.repo` is corrupted, it will not sync over to the source directory.

To resolve this problem, you need to sometimes tell `repo` to forcefully sync the repository in `.repo` again with the remote repository so that changes can be synced over to the main source tree. Use the `--force-sync` flag to achieve this:

    repo sync --force-sync

#### Why not use `-f` as well?

Starting from `repo` 1.26, the use of the flags `-f` and `--force-sync` together has been deprecated. `repo` will warn you of this change if you try and use those two flags together. To make sure your scripts do not break in future `repo` versions, it is recommended to not use the `-f` flag at all.

### `reposync` alias

    repo sync -c -j$(nproc --all) --no-clone-bundle --no-tags

That's quite long! How about we add this to our `.bashrc` as a alias? That way, we only have to type one phrase for `bash` to automatically type that out for us.

Open up `~/.bashrc` and add these lines:

    # Alias to sync
    alias reposync='repo sync -c -j$(nproc --all) --no-clone-bundle --no-tags'

This way, next time you want to sync, just type `reposync` and `bash` will substitute the command for you. Easy! Just don't forget to `source ~/.bashrc` otherwise `bash` will not know of this new alias.

## Delete all device trees and local manifests

**WARNING**: If you have any changes in your device trees, commit them and push them to a remote repository. This tip will permanently delete your local changes, so back them up!

While messing around with device specific folders, you may break something and the build process might not work. Or, you may have multiple devices synced and you want to delete it all and start over. This tip/command will let you delete only the device trees.

Add this function to your `~/.bashrc`:

    # Remove all device trees/local manifests
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

After running `resettree`, make sure to initialize a new tree by running `breakfast <devicename>`.

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