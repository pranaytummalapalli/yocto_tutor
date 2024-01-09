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

### 
