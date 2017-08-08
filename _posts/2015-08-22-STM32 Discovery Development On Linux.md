---
layout: post
comments: true
categories: 单片机相关
---

转自[wolinlabs](http://www.wolinlabs.com/blog/linux.stm32.discovery.gcc.html)

## Summary

The STM32F4Discovery is a $15 development board, featuring a 168Mhz ARM Cortex M4 (STM32F407VGT6) The ARM is programmed via an STLINK/V2 interface connected to a PC's USB port (i.e. requires no JTAG plug, ICD, BDM, proprietary dongle, etc.)

There are four different Windows compilers+IDEs mentioned for use with the board in STM's materials. These are evaluation versions, limited in executable size, with the licensed versions generally costing more than a student/hobbyist would like to spend. This article explains show to use ARM's free GNU Tools for ARM Embedded Processors toolchain, a port of GCC maintained by Advanced RISC Machines (ARM Ltd) for the Cortex-R/M families for "bare metal development" which includes glibc support.

These instructions have been tested for Ubuntu 12.04 LTS, GNU Tools for ARM Embedded Processors 4.8-2014q2, and 2013.03.06 commit of STLINK. They may also work for other versions.


## Allow Users Access to USB Devices

By default, only the superuser has access to USB devices. Change things so that you have access to the F4 Discovery USB STLINK device:

Create a new udev rule in /etc/udev/rules/45-usb-stlink-v2.rules:

    SUBSYSTEM=="usb", ATTR{idVendor}=="0483", ATTR{idProduct}=="3748", MODE="0666"

The easiest thing to do at this point is reboot. Or you can try:

sudo service udev restart
Disconnect the F4 Discovery USB cable and reconnect it

## Download the GNU/ARM toolchain

I am using the GNU Tools for ARM Embedded Processors toolchain. This GNU toolchain is maintained by ARM for the Cortex-R/M families for "bare metal development." It includes glibc support.

On the download page, grab the Linux current installation tarball. Download this to your home directory and unpack it using something like this, but get the current release if there is a newer one.

    tar -xvjf gcc-arm-none-eabi-4_8-2014q2-20131204-linux.tar.bz2

The toolkit is now in the ~/gcc-arm-none-eabi-4_8-2014q2 directory. Add this directory to the end of the path with something like:

    export PATH=$PATH:~/gcc-arm-none-eabi-4_8-2014q2/bin

As a sanity check, run the compiler to display the version. You should see something like this:

    $ arm-none-eabi-gcc --version
    arm-none-eabi-gcc (GNU Tools for ARM Embedded Processors) 4.8.4 20140526 (release)
    [ARM/embedded-4_8-branch revision 211358]
    Copyright (C) 2013 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.


You need to add the toolchain directory to the search path in any console you are building in, so I'd recommend adding it to your ~/.bashrc or some convenient script you run to set up the build environment, etc


You may also need to install automake for builds to work:

    sudo apt-get install automake

## Download and Build STLINK

The F4 Discovery board uses an interface called STLINK for programming and debugging firmware. The software that talks to the STLINK interface is also called STLINK.

In order to retreive and build STLINK, you'll first need to install a couple of packages:

    sudo apt-get install autoconf pkg-config libusb-1.0 git

Retreive a copy of the STLINK source:

    cd ~
    git clone https://github.com/texane/stlink.git


Build the code following the instructions in the README, with:

    cd ~/stlink
    ./autogen.sh
    ./configure
    make


As a sanity check, look in ~/st-link and you should see st-util, st-flash, etc...

## Download STM32 Libraries and Blinky Example

Download my modified version of the STM library code and blinky example project

    cd ~
    git clone https://github.com/rowol/stm32_discovery_arm_gcc


If you want the unmodified STM library source download that from STM's website (I've added Makefiles and linker files to build STM's example projects w/ the GNU/ARM toolchain, but left everything else intact..)

## Compile Your Source Code

It's not important how this app works, the idea is just to get something that works, then try building it and programming it to the F4 Discovery board so that we can verify the build/program/run sequence works.

The important part of the project is the Makefile

Edit the Makefile, and set STLINK to point to where you have installed it:

    STLINK=/home/rowol/stlink             #Example, change to your location

Make STM_COMMON point to where you installed the STM32 Library code:

    STM_COMMON=/home/rowol/stm32_discovery_arm_gcc/STM32F4-DISCOVERY_FW_V1.1.0

Build the code by running make:

    make

## Program ARM's Flash and Run Code

There are at least two possible ways to program the flash

#### Makefile

    make burn

The burn target programs the flash using st-flash. After the flash programming completes, press the black reset button on the F4 Discovery and the code will run.

#### GDB (debugger)

In the directory with your source code, I've also add a .gdbinit file which contains:

    define reload
    kill
    monitor jtag_reset
    load
    end

    target extended localhost:4242
    load
This will execute whenever you start GDB in the source directory

To start up:

    ~/stlink/st-util                 #Start GDB server
    arm-none-eabi-gdb main.elf       #In a second terminal window, start GDB

    continue                         #Starts code running, or use other GDB commands to
                                     # set breakpoints, etc first...

If you need to make changes and recompile the code, you can reload the same application (i.e. .bin file) from within GDB with this sequence:
Hit ^C if the code is running, to get to a gdb prompt
reload      #Macro we defined in .gdbinit, it does kill/monitor/jtag_reset/load sequence
Start new code running again with 'run' or other debugger commands

## Links

[STM32F4 Discovery Kit Resource Page](http://www.st.com/web/catalog/tools/FM116/SC959/SS1532/PF252419)  
[CMSIS - Cortex Microcontroller Software Interface Standard](http://www.arm.com/products/processors/cortex-m/cortex-microcontroller-software-interface-standard.php)  
[USB OTG Host and Device Library User Manual](http://www.wolinlabs.com/blog/cCD00289278.pdf)  

*Send comments, questions, money in large denominations, etc to eng at mysticengineering.com*
