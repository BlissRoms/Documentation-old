# Taking bug reports

!!! warning
    If you were redirected here from Telegram, **please** read this entire page in full! We cannot help you if you submit an incomplete bug report.

## What information is required?

As stated in the XDA post and various "Update" posts, we require some debugging information in order to help troubleshoot your issue.

When you are asking for help on any of the platforms we are on, such as Telegram or XDA, we ask that you provide the following information:
 - Device details (as much info about your device as possible)
 - The exact filename of the Bliss OS `.iso` file used for installation
 - Logs from Android `logcat`

The last bit is **extremely important**! We're not psychics, and certainly not psychics capable of debugging binary issues over the air. Without logs, we don't know what your device is trying to do, or rather, failing to do. So here is a brief rundown on the process of capturing logs.

## Capturing logcat

!!! question
    **Q: Can I follow the instructions below, but use local terminals?**

    Yes, but **only** if you have **root permissions**. We include a default terminal app that you can enable by going to Developer Options. There's also a Terminal Emulator app available on Google Play.

If you would rather not use a terminal app, you can use another virtual console (`tty`) in order to troubleshoot. Linux has multiple virtual consoles that you can navigate by using shortcuts. On Bliss OS, we use the Alt key and the corresponding function keys (F1 for root terminal, F7 for GUI, etc.) to navigate those virtual consoles.

Start off by entering the root terminal (Alt-F1). Then type:

    su
    logcat > sdcard/logcat.txt

Then come back to the GUI (Alt-F7) and repeat what caused your issue. Once done, return to the root terminal and end the log collection by pressing Ctrl-C.

Then grab the `logcat.txt` file from your root directory (`/sdcard`) and share it with us in order for us to help you!

## Drivers

If your device already runs well on a Linux distro, sharing information about that distro with us may help us debug driver issues.

## I'm done, where should I send this to?

Our [Telegram chat for Bliss OS would be a good place to send bug reports!](https://t.me/blissx86)

We are working on an official bug-tracker, it's not ready just yet. If we make it available we will update the documentation!