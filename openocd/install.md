# OpenOCD install

## Contents
1. [Overview](#overview)
1. [Documentation](#documentation)
1. [Windows](#windows)
1. [OS X](#os-x)
1. [GNU/Linux](#gnulinux)
1. [Testing](#testing)

## Overview

[OpenOCD](http://openocd.sourceforge.net) is an open source project hosted on [SourceForge](https://sourceforge.net/projects/openocd/), and project maintainers insist that all end-users should compile it from the latest version of the source code available from their repository. There are no special stable branches or tags and there are no clear release dates for future versions. On Windows you need to install MSYS2 and use the appropriate package build procedure.

If you are like us and consider that professional solutions should include stable, hassle free binary distributions, you might be interested to know that a custom packed version of OpenOCD, based on the latest public version (0.8.0) was added to GNU ARM Eclipse, as **GNU ARM Eclipse OpenOCD**. It can be downloaded from the <a href="https://sourceforge.net/projects/gnuarmeclipse/files/OpenOCD/" target="_blank">SourceForge files section</a>.

So, if you are not interested in building from sources, and appreciate a better integration with the Eclipse plug-in, please feel free to use the GNU ARM Eclipse OpenOCD binaries, and preferably install them in the default locations.

**Warning:** OpenOCD is a very complex project, capable of working with many JTAG probes, but support for them must be explicitly included at build time, so be sure that support for your JTAG probe was included in the binaries you plan to use. The **GNU ARM Eclipse OpenOCD** includes support for most existing probes.

## Documentation

The OpenOCD documentation is available in the <a href="https://sourceforge.net/projects/openocd/files/openocd/" target="_blank">Files</a> section of the SourceForge project. For version 0.8.0, the manual is <a href="http://sourceforge.net/projects/openocd/files/openocd/0.8.0/openocd.pdf" target="_blank">openocd.pdf</a>.

## Windows

For user convenience, the Windows versions of **GNU ARM Eclipse OpenOCD** are packed as Windows setup wizards. Go to the <a href="https://sourceforge.net/projects/gnuarmeclipse/files/OpenOCD/" target="_blank">SourceForge files section</a> and download the latest version named like:

  * **gnuarmeclipse-openocd-win64-0.8.0-*-setup.exe**
  * **gnuarmeclipse-openocd-win32-0.8.0-*-setup.exe**

Select the win64 file for Windows x64 machines and the win32 file for Windows x32 machines.

Run the setup as usual.

![The OpenOCD Setup.](https://github.com/gnuarmeclipse/openocd/wiki/images/2015/openocd-setup-wizard.png)

It is recommended to keep the default install location:

![Accept the default OpenOCD destination folder.](https://github.com/gnuarmeclipse/openocd/wiki/images/2015/openocd-setup-destination.png)

The default install location is:

* `C:\Program Files\GNU ARM Eclipse\OpenOCD\*`

The result is a structure like:

![The OpenOCD folders structure.](https://github.com/gnuarmeclipse/openocd/wiki/images/2015/windows-folders-openocd.png)

To check if OpenOCD starts, use the following command:

```
C:>"C:\Program Files\GNU ARM Eclipse\OpenOCD\0.8.0-201503201840\bin\openocd" --version 
GNU ARM Eclipse 64-bit Open On-Chip Debugger 0.8.0-00036 (2015-03-20-18:40)
```

### Drivers

As usual on Windows, mastering drivers is a challenge and OpenOCD is no exceptions, so don't be surprised to encounter many incompatible drivers for various JTAG probes. The OpenOCD distribution includes some libusb drivers, and recommends to run the zadig.exe tool to activate them. Although this manoeuvre will make OpenOCD happy, it will most probably ruin other USB drivers you might have installed. Our strong recommendation is NOT to do this, and instead use the manufacturer drivers, when compatible with OpenOCD.

#### ST-LINK/V2

One example of compatible drivers are the ST-LINK/V2 USB drivers, from ST, available as part number [STSW-LINK003](http://www.st.com/web/catalog/tools/FM147/SC1887/PF258167) or [STSW-LINK006](http://www.st.com/web/catalog/tools/FM147/SC1887/PF259459) for Windows 8. Download the s-link\_v2\_usbdriver.zip archive and run the **st-link\_v2\_usbdriver.exe** with administrative privileges. By default, the package will install the drivers to:

*  `C:\Program Files\STMicroelectronics\st_toolset`

As for most Windows drivers, to complete the installation, a restart will help.

Connect the ST-LINK/v2 or the DISCOVERY board and check in **Control Panel** → **System** → **Device Manager** if the JTAG is operational.

![ST-LINK Windows device.](https://github.com/gnuarmeclipse/openocd/wiki/images/2015/windows-devices-stlink.png)

For other probes follow the manufacturer instructions.

Note 1: All Windows tests were performed on Windows 7. If, for any reasons, you still use the venerable Windows XP, some differences may be observed in the USB subsystem; to stay on the safe side, try to always use original manufacturer drivers.

Note 2: OpenOCD v0.7.0 does not work with the current J-Link drivers, so on Windows it is not possible to use OpenOCD with J-Link; use the SEGGER supplied software instead.

#### Zadig

For some devices, for example [ARM-USB-OCD](https://www.olimex.com/Products/ARM/JTAG/) from [Olimex](https://www.olimex.com/), after installing the vendor drivers, you must also install [Zadig](http://zadig.akeo.ie) and convert the vendor drivers to WinUSB drivers.

## OS X

For user convenience, the OS X version of **GNU ARM Eclipse OpenOCD** is packed as an OS X install package. Go to the <a href="https://sourceforge.net/projects/gnuarmeclipse/files/OpenOCD/" target="_blank">SourceForge files section</a> and download the latest version named like:

*  **gnuarmeclipse-openocd-osx-0.8.0-*.pkg**

As the name implies, this is an OS X package, that can be installed on any recent 64-bit OS X (it was tested on 10.9 and 10.10).

Run the install as usual.

![Install the OS X package.](https://github.com/gnuarmeclipse/openocd/wiki/images/2015/openocd-mac-installer.png)

The package is installed in:

* `/Applications/GNU ARM Eclipse/OpenOCD/*`

To check if OpenOCD starts, use:

```
$ /Applications/GNU\ ARM\ Eclipse/OpenOCD/0.8.0-201501181257/bin/openocd --version
GNU ARM Eclipse 64-bit Open On-Chip Debugger 0.8.0-00036 (2015-01-18-12:57)
```

## GNU/Linux

The GNU/Linux versions of **GNU ARM Eclipse OpenOCD** are packed as TGZ archives. Go to the <a href="https://sourceforge.net/projects/gnuarmeclipse/files/OpenOCD/" target="_blank">SourceForge files section</a> and download the latest version named like:

* **gnuarmeclipse-openocd-debian64-0.8.0-201501181055.tgz**
* **gnuarmeclipse-openocd-debian32-0.8.0-201501181055.tgz**

As the name implies, these are Debian **tar.gz** archives, but can be executed on most recent GNU/Linux distributions (they were tested on Debian, Ubuntu, Manjaro, SuSE and Fedora). Select the debian64 file for 64-bit machines and the debian32 file for 32-bit machines.

In case you use an older distribution and encounter difficulties to run **GNU ARM Eclipse OpenOCD**, you can also try to build it from sources on your machine. A special script was developed for this; with minor changes it should run on all GNU/Linux distributions. Details about this script are available in the <a href="http://gnuarmeclipse.livius.net/wiki/Main_Page#OpenOCD" target="_blank">project wiki</a>. As a last resort, if your distribution includes an OpenOCD package, you can install it using the specific tools.

To install this package, unpack the archive and copy it to:

* `/opt/gnuarmeclipse/openocd/*`

Note: although perfectly possible to install it in any location, it is recommended to use this location, since by default the plug-in searches for the executable in this location.

To check if OpenOCD starts and is recent, use:

```
$ /opt/gnuarmeclipse/openocd/0.8.0-201501181257/bin/openocd --version
GNU ARM Eclipse 64-bit Open On-Chip Debugger 0.8.0-00036 (2015-01-18-12:57)
```

### UDEV

For the JTAG probes implemented as USB devices (actually most of them), the last installation step on GNU/Linux is to configure the UDEV subsystem. OpenOCD provides an UDEV rules file defining all the supported IDs; to install it, just copy the file to /etc/udev/rules.d and eventually notify the daemon:

```
sudo cp /opt/gnuarmeclipse/openocd/0.8.0-201501181257/contrib/99-openocd.rules \
 /etc/udev/rules.d/
sudo udevadm control --reload-rules
```

Note: If you previously installed the J-Link binaries, the USB IDs were already added to UDEV. The above OpenOCD rules file also defines the J-Link ID. Apparently UDEV does not complain; if you encounter problems, just comment out the definition in the OpenOCD file.

## Testing

To test if OpenOCD is able to connect to a specific board, you generally need to select the interface and the processor. As a shortcut, for some well known boards, there are ready made configuration files to set both the interface and the processor. For example, on OS X, to test a connection via ST/LINK v2 to the STM32F4DISCOERY board, you can use the sample below:

```
/Applications/GNU\ ARM\ Eclipse/OpenOCD/0.8.0-201501181257/bin/openocd -f board/stm32f4discovery.cfg
GNU ARM Eclipse 64-bit Open On-Chip Debugger 0.8.0-00036-gb7535dd (2015-01-18-12:57)
Licensed under GNU GPL v2
For bug reports, read

http://openocd.sourceforge.net/doc/doxygen/bugs.html

srst_only separate srst_nogate srst_open_drain connect_deassert_srst
Info : This adapter doesn't support configurable speed
Info : STLINK v2 JTAG v14 API v2 SWIM v0 VID 0x0483 PID 0x3748
Info : using stlink api v2
Info : Target voltage: 2.905638
Info : stm32f4x.cpu: hardware has 6 breakpoints, 4 watchpoints
^C
```

## Next

  * [How to use the OpenOCD debugging Eclipse plug-in][1]

 [1]: /blog/openocd-debugging/ "The OpenOCD debugging Eclipse plug-in"