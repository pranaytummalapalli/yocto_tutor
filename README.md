# Yocto Fundamentals

"It’s not an embedded Linux distribution,
it creates a custom one for you."

The Yocto Project (YP) is an open source collaboration project that helps developers create custom Linux-based systems regardless of the hardware architecture.

The project provides a flexible set of tools and a space where embedded developers worldwide can share technologies, software stacks, configurations, and best practices that can be used to create tailored Linux images for embedded and IOT devices, or anywhere a customized Linux OS is needed.

Whether you want an out of the box software experience on your embedded target or need to optimise for performance, yocto is a great tool to achieve a customised linux based OS. Its possible to develop custom drivers for the peripherals you will use for your specific custom hardware and integrate with the OS as an end-product.

## Pre-requisites

To get started with Yocto to build a custom embedded distribution you need to have:

            Host computer  -----------> Client Target embedded computer
            (PC with atleast                  (Embedded SBCs)
                8GB RAM)

### Minimum Free Disk Space

To build an image such as core-image-sato for the [qemux86-64 machine](https://docs.yoctoproject.org/dev-manual/qemu.html "Using QEMU"), you need a system with at least 90 Gbytes of free disk space. However, much more disk space will be necessary to build more complex images, to run multiple builds and to cache build artifacts, improving build efficiency.

### Minimum System RAM

You will manage to build an image such as core-image-sato for the qemux86-64 machine with as little as 8 Gbytes of RAM on an old system with 4 CPU cores, but your builds will be much faster on a system with as much RAM and as many CPU cores as possible.

### Supported Linux Distributions

Currently, the 4.3.999 release (“Nanbield”) of the Yocto Project is supported on the following distributions:

- Ubuntu 20.04 (LTS)
- Ubuntu 22.04 (LTS)
- Fedora 38
- CentOS Stream 8
- Debian GNU/Linux 11 (Bullseye)
- Debian GNU/Linux 12 (Bookworm)

For more details on System specifications visit the [System Requirements documentation.](https://docs.yoctoproject.org/ref-manual/system-requirements.html)

## Basics and Concepts

### Poky

Poky (pronounced Pock-ee. also, named after the poky stick snacks), is a reference linux based embedded distribution and a reference test configuration. It provides the following:
- A base-level functional distro used to illustrate how to customize a distribution.
- A means by which to test the Yocto Project components (i.e. Poky is used to validate the Yocto Project).
- A vehicle through which you can download the Yocto Project.

Poky is a starting point to build your custom embedded distribution and serves as a reference template.

### Open Embedded (OE)

This is the core build system for the Yocto Project that utilises BitBake as its central build and meta-data maintenance tool.

### BitBake

BitBake is a generic task execution engine that allows shell and Python tasks to be run efficiently and in parallel while working within complex inter-task dependency constraints. One of BitBake’s main users, OpenEmbedded, takes this core and builds embedded Linux software stacks using a task-oriented approach.

Conceptually, BitBake is similar to GNU Make in some regards but has significant differences:

BitBake executes tasks according to the provided metadata that builds up the tasks. Metadata is stored in recipe (.bb) and related recipe “append” (.bbappend) files, configuration (.conf) and underlying include (.inc) files, and in class (.bbclass) files. The metadata provides BitBake with instructions on what tasks to run and the dependencies between those tasks.

BitBake includes a fetcher library for obtaining source code from various places such as local files, source control systems, or websites.

The instructions for each unit to be built (e.g. a piece of software) are known as [“recipe”](#Recipes) files and contain all the information about the unit (dependencies, source file locations, checksums, description and so on).

BitBake includes a client/server abstraction and can be used from a command line or used as a service over XML-RPC and has several different user interfaces.

### Meta-data

In Yocto Project, metadata refers to the collection of configuration files, recipes, and other components that define how a software image is built for an embedded system. The metadata provides the necessary instructions and information for the OpenEmbedded build system to fetch, compile, and package software components into a target image.

### Layers

Layers are repositories that contain related sets of instructions that tell the OpenEmbedded Build System what to do. You can collaborate, share, and reuse layers.

Layers can contain changes to previous instructions or settings at any time. This powerful override capability is what allows you to customize previously supplied collaborative or community layers to suit your product requirements.

You use different layers to logically separate information in your build. As an example, you could have BSP, GUI, distro configuration, middleware, or application layers. Putting your entire build into one layer limits and complicates future customization and reuse. Isolating information into layers, on the other hand, helps simplify future customizations and reuse. You might find it tempting to keep everything in one layer when working on a single project. However, the more modular your Metadata, the easier it is to cope with future changes.

### Recipes

A set of instructions for building packages. A recipe describes where you get source code, which patches to apply, how to configure the source, how to compile it and so on. Recipes also describe dependencies for libraries or for other recipes. Recipes represent the logical unit of execution, the software to build, the images to build, and use the .bb file extension.

### Source Directory

The Source Directory contains BitBake, Documentation, Metadata and other files that all support the Yocto Project. Consequently, you must have the Source Directory in place on your development system in order to do any development using the Yocto Project.

NOTE: This is practically the same as the poky directory in your filesystem.

### bblayers.conf

### local.conf

The local.conf file is a configuration file where all local configurations and user settings are stored. This also stores information about the target build system like architecture settings etc.

- MACHINE variable:
- 

### Images

## Getting Started (Quick Build)

This documentation follows aspects from the [Official Yocto Documentation](https://docs.yoctoproject.org/index.html) as well as [Yocto Tutorials Series](https://www.youtube.com/playlist?list=PLwqS94HTEwpQmgL1UsSwNk_2tQdzq3eVJ) by Tech-A-Byte. I have mostly referred to Tech-A-Byte's playlist for the quick build and basic concepts section but also referred to the official documentation where required (which i shall mention where used).

### Software Required

#### 1. Balena Etcher.io

Etcher.io will be used to flash the image onto the SD Card. It can be downloaded from the Balena Etcher [downloads section](https://etcher.balena.io/#download-etcher).

Download the 64-BIT x64 Linux AppImage if you are using 18.04 or higher.
Then right click the appimage and go to properties. There under the permissions tab select the "Allow executing file as program" check box.

Alternatively, etcher can also be installed from commandline.

1. **Add debian repository for etcher:**

    Add Etcher’s repository to the list of repositories from which Ubuntu retrieves application files. In the console, type the following command:

    ```bash
    echo "deb https://deb.etcher.io stable etcher" | sudo tee /etc/apt/sources.list.d balena-etcher.list
    ```

2. **Add the repository key to authenticate the package source**

    Next, use apt-key to add Etcher’s repository key to the trusted list. To do this, type the following:

    ```bash
    sudo apt-key adv --keyserver hkps://keyserver.ubuntu.com:443 --recv-keys 379CE192D401AB61
    ```

3. **Install etcher**

    Now you can proceed with installing Etcher using apt.

    1. First, update the package list:

        ```bash
        sudo apt update
        ```
    2. Next, install the package:

        ```bash
        sudo apt install balena-etcher-electron
        ```

    3. When prompted, type **y** and press **Enter**.

    4. Wait for the installation to finish and run Etcher by finding it in the list of your Ubuntu applications. Alternatively, start it by typing **balena-etcher-electron** in the command line.

### Build Packages Required By Host

You must install essential host packages on your build host. The following command installs the host packages based on an Ubuntu distribution:

```bash
sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev python3-subunit mesa-common-dev zstd liblz4-tool file locales libacl1

sudo locale-gen en_US.UTF-8
```

### Setup Your Workspace (optional)

Setting up a separate workspace for your Poky distribution is recommended so that version control is more organised.

In your /home,
```bash
mkdir yocto_ws && cd yocto_ws
```
Now clone the poky repository here.

### VS Code Bitbake Syntax Highlighting 

For ease of use, install the Yocto BitBake extension by MicroHobby. This will highlight all relevant BitBake syntax.

### Clone Poky Repository

```bash
git clone git://git.yoctoproject.org/poky -b kirkstone
cd poky
```
The -b option in the clone command is used to select the branch that you want to clone. Without this, you will have to do an additional checkout step to switch to the relevant branch. The latest stable branch is nanbield (yocto 4.3), however, i am using Kirkstone since the Tech-A-Byte series uses this branch.

You can regularly type the following command to keep your local in sync with the remote to get access to bug fixes and improvements to the branch.
```bash
git pull
```
in your poky directory source the open embedded build environment.

```bash
source oe-init-build-env
```
this will take you to build directory. 

Inspect the conf directory:

```bash
cd conf
ls
```

the output will conatin the following files
```console
bblayers.conf  local.conf  templateconf.cfg
```
You can find detailed description of the files in the [bblayers.conf](#bblayers.conf) and [local.conf](#local.conf) section.

### Create The Sources Directory

From your build directory, create the sources directory.

```bash
mkdir ../../sources
```
### Configuring The local.conf File

Open the local.conf file in your text editor (preferably VSC).

Comment the MACHINE ??= "qemux86-64" line and add the following line to the file:

```c
MACHINE ?= "beaglebone-yocto"
```
Go to the sources directory created before and run the follwoing command (it is ideal to open another terminal in the sources directory). copy the output.

```console
/sources$ pwd                                                                                      
/home/pranayt/em_linux/yocto_tutorials/sources
```

Define the SOURCES variable to point to the sources directory:
```c
SOURCE = "/home/pranayt/em_linux/yocto_tutorials/sources"
```

Uncomment the TMPDIR, SSTATE_DIR and DL_DIR lines and replace TOPDIR with SOURCES
```c
TMPDIR = "${SOURCE}/tmp"
DL_DIR ?= "${SOURCE}/downloads"
SSTATE_DIR ?= "${SOURCE}/sstate-cache"
```

Add the following lines to the end of the file:
```c
CONF_VERSION = "2"
RM_OLD_IMAGE = "1"
INHERIT += "rm_work"
```

For definitions, see [CONF_VERSION](#local.conf), [RM_OLD_IMAGE](#local.conf) and [INHERIT](#local.conf).

### Building The Image

If not already in the ~/poky/build directory, enter to it and enter the bitbake build command with the image type you want to build:

```bash
bitbake core-image-full-cmdline
```

This will build the commandline version of the image. For other command options, see [images](#Images).

### Flashing To The Beaglebone Black

The image created as a result of the above build process is a file with .wic format. It can be found in the following folder:

```console
/home/pranayt/em_linux/yocto_tutorials/sources/tmp/deploy/images/beaglebone-yocto
```
File name:
```console
core-image-full-cmdline-beaglebone-yocto.wic
```

