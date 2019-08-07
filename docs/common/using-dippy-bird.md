# Using `dippy-bird`

If you want to review commits quickly on our Gerrit, [use the `dippy-bird.php` script created by the WikiMedia foundation.](https://github.com/wikimedia/mediawiki-tools-dippybird/blob/master/dippy-bird.php)

Thanks to Vaughn Newman (@rwaterspf1) for the original instructions!

## Installation

Make sure you have PHP installed.

[Download the script](https://github.com/wikimedia/mediawiki-tools-dippybird/blob/master/dippy-bird.php) and put it in an easily-accessible place.

## Usage

### Reviewing commits (code-review only)

To review commits, run:

    php dippy-bird.php --username=ideaman924 --server=review.blissroms.com --port=29418 -q="status:open topic:test" -a=review --review=+1 --verify=0

This will review all commits that match the following criteria:

 - Is open for review (not closed, merged, or abandoned)
 - Has the topic `test`

And will apply +1 code-review and no verify, indicating that you have a successful build with the commits included.

### Reviewing commits (code-review and verification)

To review commits with verification, run:

    php dippy-bird.php --username=ideaman924 --server=review.blissroms.com --port=29418 -q="status:open topic:test" -a=review --review=+2 --verify=+1

This will review all commits that match the following criteria:

 - Is open for review (not closed, merged, or abandoned)
 - Has the topic `test`

And will apply +2 code-review and +1 verify, indicating that you have tested the commits on an actual device. This means that the commits are now ready for merging.