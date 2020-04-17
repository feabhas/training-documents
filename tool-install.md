# Feabhas Embedded C/C++ Programming setup

# Linux (debian based)

This install was performed using Mint 

Tools required:
1. Visual Studio Code (or any other code editor of your choice)
2. The GNU Arm Embedded Toolchain
3. Segger J-Link drivers and Ozone debugger
4. Python and the Scons build systems
5. minicom (or any text-based serial port communications program)

## Visual Studio Code (VSC)

The use of VSC is not a pre-requisite, any editor will suffice (`vi`, `emacs`, Sublime Text, etc.).

The appropriate VSC `.deb` file is available from the [following site](https://code.visualstudio.com/)

## GNU Arm Embedded Toolchain

For building the target projects, `arm-none-eabi-c++` is required and part of the `$PATH`.

Download the latest tarball from [developer.arm.com](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)

A direct link to the current version is: [gcc-arm-none-eabi-9-2019-q4-major-x86_64-linux.tar.bz2](https://developer.arm.com/-/media/Files/downloads/gnu-rm/9-2019q4/gcc-arm-none-eabi-9-2019-q4-major-x86_64-linux.tar.bz2?revision=108bd959-44bd-4619-9c19-26187abf5225&la=en&hash=E788CE92E5DFD64B2A8C246BBA91A249CB8E2D2D)
MD5: fe0029de4f4ec43cf7008944e34ff8cc

extract this tarball to your file system. For simplicity we tend to extract to $HOME (e.g. `~`). If you don't install it at `~` then adjust paths accordingly.

Next, add the GNU Arm tools into the PATH. A consistent approach is to to edit `~/.profile`, e.g.
```
$ nano ~/.profile
```
Add the following line to the end of the file:
```
PATH="$HOME/gcc-arm-none-eabi-9-2019-q4-major/bin:$PATH"
```
adjusting the path accordingly. 

rerun the `.profile`:
```
$ source ~/.profile
```
and check the path setup by typing:
```
$ arm-none-eabi-c++ -v
```

## Segger J-Link drivers and Ozone debugger

To reflash the target the Segger J-Link drivers need installing. Fort target based source-code debug we use Segger's own tool Ozone. Alternatively GDB can be used if preferred.

### J-Link drivers

First, download the **J-Link Software and Documentation Pack** from:
https://www.segger.com/downloads/jlink

The download link for the `.deb` file:
J-Link Software and Documentation pack for Linux, DEB installer, 64-bit
https://www.segger.com/downloads/jlink/JLink_Linux_x86_64.deb

Once installed, open a new Terminal and type: 
```
$ JLinkExe
SEGGER J-Link Commander V6.70c (Compiled Apr  7 2020 15:01:10)
DLL version V6.70c, compiled Apr  7 2020 15:00:59

Connecting to J-Link via USB...FAILED: Cannot connect to J-Link.
J-Link>
```
If a Feabhas target is not connected then you'll see the `FAILED: Cannot connect to J-Link`. That is okay as it demonstrates that the J-Link drivers are correctly installed. 

### Ozone - The J-Link Debugger

On the same weblink as above, scroll down and find the links for **Ozone - The J-Link Debugger**.

Use the `.deb` install for:
Ozone - The J-Link Debugger for Linux, DEB installer, 64-bit
https://www.segger.com/downloads/jlink/Ozone_Linux_x86_64.deb


To test the install open Ozone using the application search bar.

## Python and the Scons build systems

Scons is a python-based build system. It is an alternative to using `make` and `Makefiles`. The Feabhas template project is configured to be built using Scons. 

using Linux, we assume Python is already installed (v2.7 is okay).

To install scons:
```
sudo apt install scons
```

To test:
```
$ scons -v
```

## minicom

Finally, some of the exercises use the target serial port. This is connect via USB to the host system. To communicate wioth the target over serial we require a text-based serial port communications program. 

Minicom is a good tool of choice, and our projects are configured to use minicom. 

### To install minicom 
#### (Debian, Ubuntu, Mint, Kali):
```
sudo apt install minicom -y
```
#### (Fedora, CentOS, RHEL):
```
sudo yum install minicom -y
```
To test:
```
$ minicom -v
minicom version 2.7.1 (compiled Aug 13 2017)
Copyright (C) Miquel van Smoorenburg.

This program is free software; you can redistribute it and/or
modify it under the terms of the GNU General Public License
as published by the Free Software Foundation; either version
2 of the License, or (at your option) any later version.
```

# Summary

This summaries the set of tools required to run a Feabhas training course. 

* A separate document will soon be made available describes testing the setup when the physical hardware is available.
* Another document will cover testing using the simulator QEMU
* A final document will describe using Docker containers for building projects