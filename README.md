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

### BitBake

BitBake is a generic task execution engine that allows shell and Python tasks to be run efficiently and in parallel while working within complex inter-task dependency constraints. One of BitBake’s main users, OpenEmbedded, takes this core and builds embedded Linux software stacks using a task-oriented approach.

Conceptually, BitBake is similar to GNU Make in some regards but has significant differences:

BitBake executes tasks according to the provided metadata that builds up the tasks. Metadata is stored in recipe (.bb) and related recipe “append” (.bbappend) files, configuration (.conf) and underlying include (.inc) files, and in class (.bbclass) files. The metadata provides BitBake with instructions on what tasks to run and the dependencies between those tasks.

BitBake includes a fetcher library for obtaining source code from various places such as local files, source control systems, or websites.

The instructions for each unit to be built (e.g. a piece of software) are known as [“recipe”](### Recipes) files and contain all the information about the unit (dependencies, source file locations, checksums, description and so on).

BitBake includes a client/server abstraction and can be used from a command line or used as a service over XML-RPC and has several different user interfaces.

### Layers

Layers are repositories that contain related sets of instructions that tell the OpenEmbedded Build System what to do. You can collaborate, share, and reuse layers.

Layers can contain changes to previous instructions or settings at any time. This powerful override capability is what allows you to customize previously supplied collaborative or community layers to suit your product requirements.

You use different layers to logically separate information in your build. As an example, you could have BSP, GUI, distro configuration, middleware, or application layers. Putting your entire build into one layer limits and complicates future customization and reuse. Isolating information into layers, on the other hand, helps simplify future customizations and reuse. You might find it tempting to keep everything in one layer when working on a single project. However, the more modular your Metadata, the easier it is to cope with future changes.

### Recipes

A set of instructions for building packages. A recipe describes where you get source code, which patches to apply, how to configure the source, how to compile it and so on. Recipes also describe dependencies for libraries or for other recipes. Recipes represent the logical unit of execution, the software to build, the images to build, and use the .bb file extension.

### Source Directory

The Source Directory contains BitBake, Documentation, Metadata and other files that all support the Yocto Project. Consequently, you must have the Source Directory in place on your development system in order to do any development using the Yocto Project.


