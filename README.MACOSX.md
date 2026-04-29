Building Virtuoso Open Source Edition on Mac OS X
=================================================

*Copyright (C) 1998-2025 OpenLink Software <vos.admin@openlinksw.com>*


## Introduction

This document describes how to check out a copy of the git tree for development purposes on Mac OS X.

It also lists the packages that need to be installed prior to generating the necessary scripts and Makefiles to build the project.

Email questions to <mailto:vos.admin@openlinksw.com>.


## Downloading the Source Code

OpenLink Software frequently pushes updates to the Virtuoso Open Source tree on GitHub.


### Using Git on a Local Machine

Developers building Virtuoso on their own machine can make a local clone of the source tree using this command:

    $ git clone git://github.com/openlink/virtuoso-opensource.git

At this point, create your own work branch based on any of the branches available, create bugfixes and commit them to your own branch and then use the 'git format-patch' command to generate the appropriate diffs to send to <mailto:vos.admin@openlinksw.com>.


### Forking the Tree on GitHub

OpenLink Software encourages developers to create their own account on GitHub, after which they can fork the project by going to this URL with any web browser and pressing the **Fork** button:

    https://github.com/openlink/virtuoso-opensource

At this point, clone your fork to your local Mac OS X system, create your own branches to make enhancements/bugfixes, push these branches back to GitHub and then send pull requests using the GitHub interface for the OpenLink team to examine and incorporate the fixes into the master tree for an upcoming release.

GitHub has excellent documentation on how to fork a project, send pull requests, track the project etc. on:

    http://help.github.com/


### Using a Source Tar Archive

Developers not using git can also download a source tar archive from:

    https://github.com/openlink/virtuoso-opensource/archive/develop/7.x.tar.gz

This tar archive can be extracted using this command:

    $ tar xvfz virtuoso-opensource-development-7.tar.gz

Configuration and building is exactly the same as for the cloned git tree.


## Building on Mac OS X 10.10 and Above

Apple removed a number of programs from their Xcode.app command line installation including autoconf, automake, libtool, gperf and some other tools needed to build Virtuoso from a newly checked out git tree.

OpenLink Software suggests using the Homebrew package manager from http://brew.sh/ to install these tools.

After the installation of Homebrew you need to install these packages:

    $ brew install autoconf automake libtool
    $ brew install gperf bison flex git gawk pkg-config

By default, Homebrew installs all packages and libraries into subdirectories under the /usr/local directory.

Comparable packages can also be installed via MacPorts which installs into /opt/local directory.


### Optional Features

Optional features like command line editing in the isql tool require:

    $ brew install libedit

or

    $ brew install readline


### OpenSSL on Mac OS X

As Apple is actively deprecating OpenSSL from macOS, your system may have a rather old version of openssl installed; in the case of High Sierra (10.13), Apple removed the required include files from the /usr/include/openssl directory completely.

Install the OpenSSL 1.0.2 library using:

    $ brew install openssl

At configure time, use:

    $ sh ./configure \
      ..... \
      ..... \
      --enable-openssl=/usr/local/opt/openssl/


### ImageMagick on Mac OS X

The ImageMagick plugin requires these packages to be installed:

    $ brew install pkg-config
    $ brew install imagemagick@6

This installs ImageMagick 6.x together with a number of libraries to work with specific graphic formats.

At configure time, use:

    $ sh ./configure \
       ..... \
       --enable-imagemagick=/usr/local/opt/imagemagick\@6/


## Example of Running configure on Mac OS X

First set some environment variables:

    $ export CFLAGS="-O -arch x86_64"
    $ export LDFLAGS="-g"
    $ export CC="clang"

Note: On macOS 11 (Big Sur) and later set the CFLAGS to make a universal binary that runs on both Intel and Apple Silicon platforms:

    $ export CFLAGS="-O -arch arm64 -arch x86_64"

Next regenerate the configure script and all related build files, using the supplied script in your working directory:

    $ sh ./autogen.sh

Assuming this did not return an error, configure Virtuoso now.

For a full list of available `configure` options, including various optional subpackages, check the output of:

    $ sh ./configure --help

The following command includes a number of options we recommend for an initial build of Virtuoso on Mac OS X:

    $ sh ./configure \
        --enable-maintainer-mode \
        --enable-silent-rules \
        --prefix=/usr/local/vos \
        --with-layout=openlink \
        --enable-openssl=/usr/local/opt/openssl/ \
        --enable-imagemagick=/usr/local/opt/imagemagick@6/ \
        --enable-openldap \
        --disable-python \
        --with-editline \
        --with-jdk4=/System/Library/Frameworks/JavaVM.framework/Versions/1.6.0 \
        --with-jdk4_1=/Library/Java/JavaVirtualMachines/jdk1.7.0_71.jdk/Contents/Home/ \
        --with-jdk4_2=/Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home/

If these steps return without error, the archive can now be built using these commands:

    $ make

After building, optionally run the test suite to verify your binaries are in working order:

    $ make check

Finally, to install the resulting binaries into the /usr/local/vos directory:

    $ make install


## Disk Space Requirements

The build produces a demo database and Virtuoso application packages that are quite large.

At least 1.1GB of free space should be available on the build file system.

Running the testsuite requires an additional 2.8GB of free space on the system.

An installation containing the virtuoso server executable and supporting binaries, all the hosting plugins, VAD packages, config files etc excluding the database take around 350MB of disk space.


## Generate Build Files

Read the files INSTALL and README in this directory for further information on how to configure the package and install it on your system.
