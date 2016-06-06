# apt-multi-iam-s3

Additional "s3" protocol for apt so you can host your giant apt repository in s3 on the cheap!

We use this for pressflip.com to deploy and distribute all of our software.  apt is a great packaging system and s3 is a great place to backup/store static files.  apt-s3 is especially useful and fast if you are hosting your servers within EC2.

Original Author: Kyle Shank
Contributors: Cliff Moon (@cliffmoon on GH), Jens Braeuer (@jbraeuer)
Documenter: Susan Potter (@mbbx6spp on GH)

## Building

Before building this project on Ubuntu (tested on 11.10) you will need to install the following packages:

    [sudo] apt-get install libapt-pkg-dev libcurl4-openssl-dev

To build this project you simply run `make`. It will produce a binary named `s3` under the `src/` dir.

## Installing

Once compiled, the resulting s3 binary must be placed in /usr/lib/apt/methods/ along with the other protocol binaries.

Finally, this is how you add it to the /etc/apt/sources.list file if you want your credentials in the url:

    deb s3://AWS_ACCESS_ID:[AWS_SECRET_KEY_IN_BRACKETS]@s3-ENDPOINT.amazonaws.com/BUCKETNAME prod main

otherwise leave off the credentials and it will draw them from the environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_KEY_ID`.

Simply upload all of your .deb packages and Packages.gz file into the s3 bucket you chose with the file key mapping that matches the file system layout.

## Uploading repository to s3

Before synchronization, you need a s3cmd tool installed and configured:

    [sudo] apt-get install s3cmd

    s3cmd --configure

To synchronize local repository to s3 as read-only, execute:

    s3cmd sync /srv/apt-repo-dir/dists s3://bucket_name
    s3cmd sync /srv/apt-repo-dir/pool s3://bucket_name

## Using GPG keys

If you're signing you repository with key, export it to server:

    gpg --send-keys XXXXXXXX

then import it and install to apt:

    gpg --recv-keys XXXXXXXX
    gpg -a --export XXXXXXXX | [sudo] apt-key add -
