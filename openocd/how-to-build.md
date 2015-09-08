#  How to build the OpenOCD binaries

The latest version of the build script is a single run, multi-platform build, generating the Windows 32, Windows 64, GNU/Linux 32, GNU/Linux 64 and OS X distribution packages at once.

The script was developed on OS X, but it also runs on any recent GNU/Linux distribution (just that in this case it cannot generate the OS X package).

## Prerequisites

The main trick that made the multi-platform build possible is [Docker](https://www.docker.com).

Containers based on three docker images are used, one packing MinGW-w64 in a Debian 8, and two packing the basic system in Debian 7 (separate 32/64-bits containers). The more conservative Debian 7 was preferred to generate the GNU/Linux versions, to avoid problems when attempting to run the executables on older versions.

### OS X

#### Install Docker

On OS X, install the boot2docker, following the official [Install Docker on Mac OS X](https://docs.docker.com/installation/mac/) instructions.

#### Install the Command Line Tools

The OS X compiler and other development tools are packed in a separate Xcode add-on. The best place to get it is from the [Developer](https://developer.apple.com/xcode/downloads/) site, although this might require enrolling to the developer program (free of charge).

To test if the compiler is available, use:

    $ gcc --version
    Configured with: --prefix=/Applications/Xcode.app/Contents/Developer/usr --with-gxx-include-dir=/usr/include/c++/4.2.1
    Apple LLVM version 6.1.0 (clang-602.0.49) (based on LLVM 3.6.0svn)
    Target: x86_64-apple-darwin14.3.0
    Thread model: posix

#### Install MacPorts

The build procedure requires the presence of [MacPorts](http://www.macports.org). Install it according to the documentation.

#### Install required MacPorts packages

Install the following packages:

    sudo port install libtool automake autoconf pkgconfig wget
    sudo port install cmake boost libconfuse swig-python
    sudo port install texinfo texlive

### GNU/Linux

#### Install Docker

For any GNU/Linux distribution, follow the [specific instructions](https://docs.docker.com/installation/#installation).

#### Install required packages

Since most of the build is performed inside the Docker containers, there are not many requirements for the host, and most of the time these programs are in the standard distribution (**curl**, **git**, **automake**, **patch**, **tar**, **unzip**).

The script checks for them; if the script fails, install them and re-run.

### Docker images

The Docker images are available from [Docker Hub](https://hub.docker.com/u/ilegeul/). They were build using the Dockerfiles available from [ilg-ul/docker on GitHub](https://github.com/ilg-ul/docker).

## Download the build script

The script is available from the SourceForge git repository and can be [viewed online](https://sourceforge.net/p/gnuarmeclipse/se/ci/develop/tree/scripts/build-openocd.sh).

To download it use the [Download this file](https://sourceforge.net/p/gnuarmeclipse/se/ci/develop/tree/scripts/build-openocd.sh?format=raw) link. If the browser fails, use the following command in a terminal:

    curl -L https://github.com/gnuarmeclipse/build-scripts/raw/master/scripts/build-openocd.sh \
    -o ~/Downloads/build-openocd.sh

## Check the script

The script creates a temporary build **Work/openocd** folder in the the user home. Although not recommended, if for any reasons you need to change this, you can redefine WORK_FOLDER variable before invoking the script.

## Preload the Docker images

Docker does not require to explicitly download new images, but does this automatically at first use.

However, since the images used for this build are relatively large, it is recommended to load them explicitly before starting the build:

    bash ~/Downloads/build-openocd.sh preload-images

The result should look similar to:

    $ docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    ilegeul/debian      8-gnuarm-mingw      b8261b27add4        3 minutes ago       2.692 GB
    ilegeul/debian      8-gnuarm-gcc        ba65c1716b6e        12 minutes ago      1.437 GB
    ilegeul/debian      7-gnuarm-gcc        16b30d6a4244        32 minutes ago      1.6 GB
    ilegeul/debian32    7-gnuarm-gcc        2e416e412fad        43 minutes ago      1.596 GB
    ilegeul/debian      7                   a4ad8e2c6d76        4 days ago          96.13 MB
    ilegeul/debian32    7                   64980af805ad        4 days ago          93.65 MB
    debian              8                   65688f7c61c4        6 weeks ago         122.8 MB
    hello-world         latest              e45a5af57b00        4 months ago        910 B

## Build all distribution files

    bash ~/Downloads/build-openocd.sh --all

About half an hour later, the output of the build script is a set of 5 files in the output folder:

    $ ls -l output
    total 105616
    drwxr-xr-x  8 ilg  staff      272 May 11 15:28 debian32
    drwxr-xr-x  8 ilg  staff      272 May 11 15:20 debian64
    -rw-r--r--  1 ilg  staff  2313130 May 10 11:41 gnuarmeclipse-openocd-debian32-0.8.0-201505100809.tgz
    -rw-r--r--  1 ilg  staff  2313130 May 10 11:41 gnuarmeclipse-openocd-debian64-0.8.0-201505100809.tgz
    -rw-r--r--  1 ilg  staff  2274022 May 10 11:45 gnuarmeclipse-openocd-osx-0.8.0-201505100809.pkg
    -rw-r--r--  1 ilg  staff  2253926 May 10 11:29 gnuarmeclipse-openocd-win32-0.8.0-201505100809-setup.exe
    -rw-r--r--  1 ilg  staff  2285654 May 10 11:20 gnuarmeclipse-openocd-win64-0.8.0-201505100809-setup.exe
    drwxr-xr-x  8 ilg  staff      272 May 11 15:33 osx
    drwxr-xr-x  8 ilg  staff      272 May 11 15:13 win32
    drwxr-xr-x  8 ilg  staff      272 May 11 15:13 win64

## Subsequent runs

### Separate platform specific builds

Instead of **--all**, you can use any combination of:

    --win32 --win64 --debian32 --debian64 --osx

### clean

To remove all build files, use:

    bash ~/Downloads/build-openocd.sh clean

## Install hierarchy

The procedure to install GNU ARM Eclipse OpenOCD is platform specific, but relatively straight forward (a Windows setup, an OS X install or a TGZ archive on GNU/Linux). The setup/install asks no special questions, and the defaults are generally ok for most installations.

After install, this package should create structure like this (only the first two depth levels are shown):

    $ tree -L 2 /Applications/GNU\ ARM\ Eclipse/OpenOCD
    /Applications/GNU\ ARM\ Eclipse/OpenOCD
    ├── bin
    │   └── openocd
    ├── doc
    │   ├── openocd.html
    │   └── openocd.pdf
    ├── info
    │   ├── BUILD.txt
    │   ├── INFO.txt
    │   └── build-openocd-osx.sh
    ├── license
    │   ├── hidapi
    │   └── openocd
    └── scripts
        ├── bitsbytes.tcl
        ├── board
        ├── chip
        ├── cpld
        ├── cpu
        ├── interface
        ├── mem_helper.tcl
        ├── memory.tcl
        ├── mmr_helpers.tcl
        ├── target
        ├── test
        └── tools

    16 directories, 9 files

No other files are installed in any system folders or other locations.

## Uninstall

To uninstall OpenOCD from a Windows machine, use the **openocd-uninstall.exe** program.

On OS X and GNU/Linux, the GNU ARM Eclipse OpenOCD install folder is self-contained and removing it is enough for completely removing the application.

## Test

A simple test is performed by the script at the end, by launching the executable to check if all shared/dynamic libraries are correctly used.

For a true test you need to first install the package and then run the program form the final location. For example on OS X the output should look like:

    $ /Applications/GNU\ ARM\ Eclipse/OpenOCD/bin/openocd --version
    GNU ARM Eclipse 64-bits Open On-Chip Debugger 0.8.0-00022-g2628c74 (2015-01-15-20:44)
    Licensed under GNU GPL v2
    For bug reports, read
        http://openocd.sourceforge.net/doc/doxygen/bugs.html

## Stop boot2docker

On OS X, the build script automatically starts **boot2docker**, if needed.

When done, be sure you stop boot2docker, to free significant resources (a VirtualBox Ubuntu machine).

    boot2docker stop

## More build details

The script is quite complex, and an attempt to explain its functionality is in [The OpenOCD build procedures](/The_OpenOCD_build_procedures) page. For the final authoritative details, please refer to the comments available in the [script](https://github.com/gnuarmeclipse/build-scripts/raw/master/scripts/build-openocd.sh).

## Credits

Many thanks to my friend Dan Maiorescu for his major contributions to this project.