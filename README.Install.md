# Installing VOS

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Using Stable Binaries](#using-stable-binaries)
  - [Prebuilt Virtuoso binaries](#prebuilt-virtuoso-binaries)
  - [Using Docker Hub](#using-docker-hub)
  - [Using Maven Central](#using-maven-central)
- [Building VOS from Source](#building-vos-from-source)
  - [Package Dependencies](#package-dependencies)
    - [Core Dependencies](#core-dependencies)
    - [Recommended Packages](#recommended-packages)
    - [Specific Usage Cases](#specific-usage-cases)
    - [Linux](#linux)
    - [macOS](#macos)
  - [Building](#building)
  - [Diskspace Requirements](#diskspace-requirements)
- [Starting Virtuoso](#starting-virtuoso)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Using Stable Binaries

### Prebuilt Virtuoso binaries

For convenience, you can download prebuilt binaries for most Linux,
macOS and Windows platforms from [the latest stable release on Github](https://github.com/openlink/virtuoso-opensource/releases/latest).

Previous releases are also backed-up on
[Sourceforge](https://sourceforge.net/projects/virtuoso/files/virtuoso/).

### Using Docker Hub

For more information about using the
the Virtuoso Docker image, visit [Docker Hub](https://hub.docker.com/repository/docker/openlink/virtuoso-opensource-7/general).

### Using Maven Central

Virtuoso JDBC drivers, as well as Jena, Sesame
and RDF4j providers, can be downloaded from [Maven Central](https://central.sonatype.com/search?q=com.openlinksw).

## Building VOS from Source

Windows requires special instructions. Please see
[README.Windows.md](README.Windows.md) for details.

First, checkout a clone of the git repository:

```
$ git clone https://github.com/openlink/virtuoso-opensource.git
$ cd virtuoso-opensource
```

The default and recommended branch is `develop/7`. Users requiring a
slower update cycle may prefer `stable/7` instead.

### Package Dependencies

#### Core Dependencies

For a minimal build with core VOS functionality, ensure you have the following packages and recommended versions installed:

| Package   | Minimum | Upto   | From                                  |
| --------- | ------- | ------ | ------------------------------------- |
| autoconf  | 2.57    | 2.69   | http://www.gnu.org/software/autoconf/ |
| automake  | 1.9     | 1.16.1 | http://www.gnu.org/software/automake/ |
| libtool   | 1.5     | 2.4.6  | http://www.gnu.org/software/libtool/  |
| flex      | 2.5.33  | 2.6.4  | http://flex.sourceforge.net/          |
| bison     | 2.3     | 3.5.1  | http://www.gnu.org/software/bison/    |
| gperf     | 3.0.1   | 3.1    | http://www.gnu.org/software/gperf/    |
| gawk      | 3.1.1   | 5.3.0  | http://www.gnu.org/software/gawk/     |
| m4        | 1.4.1   | 1.4.18 | http://www.gnu.org/software/m4/       |
| make      | 3.79.1  | 4.2.1  | http://www.gnu.org/software/make/     |
| OpenSSL   | 0.9.8e  | 3.3.x  | http://www.openssl.org/               |

The `Minimum` and `Upto` columns specify known-good versions for each
package. Other versions, both older and newer, *might* work, but are
untested and could cause problems.

You can test the installed version of each package by running it with
`--version` except openssl, where the command is `openssl version`.

#### Recommended Packages

In addition to the above core, we recommend the following:

| Package | Recommended Version | From                               |
| ------- | ------------------- | ---------------------------------- |
| libedit | 20240517-3.1        | https://www.thrysoee.dk/editline/  |
| bzip2   | 1.0.8               | https://sourceware.org/pub/bzip2/  |
| xz      | 5.2.12              | https://tukaani.org/xz/            |

* libedit makes the `isql` commandline utility more usable.
* bzip2 and xz are useful for bulk-loading large amounts of RDF data.

#### Specific Usage Cases

The following packages are required if you have specific use-cases
in mind:

| Package     | Recommended Version | From                                 |
| ----------- | ------------------- | ------------------------------------ |
| openldap    | >= 2.6.8            | https://www.openldap.org/            |
| proj        | == 4.9.3            | https://proj4.org/                   |
| geos        | == 3.5.1            | https://www.osgeo.org/projects/geos/ |
| shapefileio | == 1.6.0            | https://shapelib.maptools.org/       |
| imagemagick | >= 6.9.13           | https://download.imagemagick.org/    |

Virtuoso has support for GeoSPARQL functions which requires the `proj4`,
`geos` and `shapefileio` plugins. Building these is the subject of a
separate document: [README.GeoSPARQL.md](README.GeoSPARQL.md).

Install the required dependencies - pick one of the following lines for
your OS / distribution and then resume at the "Building" section below:


#### Linux

<details>
<summary>On Ubuntu or Debian</summary>

```
$ sudo apt install build-essential autoconf automake libtool flex bison gperf gawk m4 make libssl-dev
```

Optionally:
```
$ sudo apt install libreadline-dev libbz2-dev liblzma-dev
```
</details>


<details>
<summary>On Alma or Rocky Linux</summary>

```
$ sudo yum install autoconf automake gcc libtool flex bison gperf gawk m4 make openssl openssl-devel
```

Optionally:
```
$ sudo yum install bzip2-devel xz-devel libedit-devel
```
</details>

<details>
<summary>On RedHat Enterprise Linux</summary>
<br/>

On RedHat Linux 7, first add this repository:
```
$ sudo yum --enablerepo=rhui-REGION-rhel-server-optional install gperf
```

On RedHat Enterprise Linux 8 (and newer), first add this repository:

```
$ sudo yum --enablerepo=PowerTools install gperf
```

And on all versions, install all dependencies as follows:

```
$ sudo yum install autoconf automake gcc libtool flex bison gperf gawk m4 make openssl openssl-develop
```
</details>

<details>
<summary>On OpenSuSE</summary>

```
$ sudo zypper install autoconf automake gcc libtool flex bison gperf gawk m4 make openssl libopenssl-3-devel
```

Optionally:
```
$ sudo zypper install libedit-devel libbz2-devel xz-devel
```
</details>

#### macOS

<details>
<summary>On macOS</summary>

First, install [Xcode](https://developer.apple.com/xcode/) using the
Mac App Store.

Second, run the following to install the command line developer tools:

```xcode-select --install```

Third, many of the above utilities were removed from Xcode so you should
install [Homebrew](https://brew.sh/), then run

```
brew install autoconf automake gcc libtool flex bison gperf gawk m4 make openssl@3.0
```
</details>

-------

### Building

To (re)generate the configure script and all related build files, use
the supplied script in your working directory:

```
$ ./autogen.sh
```

Assuming this runs successfully,


```
$ ./configure --enable-maintainer-mode --prefix=/opt/virtuoso-opensource --with-layout=openlink
```

   * on macOS, add a pointer to find OpenSSL from homebrew:
```
$ ./configure --enable-maintainer-mode --prefix=/opt/virtuoso-opensource --with-layout=openlink --enable-openssl=/opt/homebrew/Cellar/openssl@3.0/3.0.15/
```

and then

```
$ make -j4
```

To run the test suite (optional):

```
$ make check
```

and finally

```
$ sudo make install
```

------

VOS can be configured with many options and VAD packages, for example
libshape, libgeos, libproj, ImageMagick and other hosting modules. Many
of these will require further development versions of supporting libraries
of their own.

For further information, run:

```
$ ./configure --help
```


Certain build targets are only enabled when running configure with the `--enable-maintainer-mode` flag.

The installer supports multiple different filesystem layouts through the `--with-layout=` option.


-----

### Diskspace Requirements

The build produces a demo database and Virtuoso application packages that
are quite large. At least 1.2 GiB of free space should be available in
the build file system.

When running `make install', the target file system should have about 300
MB free. By default, the install target directories are under /usr/local/.

Running the testsuite will require approximately 2 GiB extra diskspace.

The minimum working configuration consists of the server executable
and config files plus database, no more than a few MB for the server
executable, depending on platform and options.


## Starting Virtuoso

Change into the installation's `database/` subdirectory and start
the server:

```
$ cd /opt/virtuoso-opensource/database

$ ls
virtuoso.ini
```

For the first run, to illustrate the potential console output, we run
it in debug foreground mode:

```
$ ../bin/virtuoso-t -df
```
The server will take a few seconds to start, install the Conductor VAD
and eventually say it's online:

```
...
15:14:08 INFO: PL LOG: Installation with dependencies complete
15:14:08 INFO: Checkpoint started
15:14:08 INFO: Checkpoint finished, log reused
15:14:08 INFO: HTTP/WebDAV server online at 8890
15:14:08 INFO: Server online at 1111 (pid 50403)
```

In normal use, the server can be run without any arguments
```
../bin/virtuoso-t
```
and it will detach from the console, leaving its output only in
virtuoso.log.

At this point, you can open http://localhost:8890/ in your
browser. This is the welcome home page with links to Conductor
(`http://localhost:8890/conductor`) and the SPARQL endpoint
(`http://localhost:8890/sparql`).

Additionally, from the terminal, you can also run
```
../bin/isql
```
for the SQL interface (port 1111 by default).
