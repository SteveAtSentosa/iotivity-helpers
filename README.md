Building and Running IoTivity
=============================

This guide describes how to:

* Create and manage an Ubuntu VM within which iotivity can be built and run
* Download, build, and run IoTivity sample clients and servers
* Run The OCF CTT certification suite against IoTivity

The environment outlined in this guide is focused on running the CTT and IoTivity on the same machine, the specifics of which are:

* Computer running windows
* Ubuntu 16.04 as a VM on VirtualBox

This guide assumes that you have basic familiarity with setting up a VM and with Ubuntu

## Create an Ubuntu VM

1. Download and install the latest version of [Virtual Box](https://www.virtualbox.org/)

2. Download your preffered version of an [Ubuntu desktop .iso file](https://www.ubuntu.com/download/desktop) targeted for amd/i386, for example `ubuntu-16.04.3-desktop-amd64.iso`.  (This guide was validated against v16.04)

3. Create an Ubuntu VM as follows

    * at least 2048 MB RAM
    * a fixed size drive of at least 20 GB
    * The first time you start the VM, you will be promted for the startup disk.  Click the folder icon on the bottom right of the dialog, and select the Ubuntu .iso file that you downloaded earlier, then hit **Start**
    * At the next prompt, hit the **Install Ubuntu** and finish the configuration as you desire


4. Prepare your Ubuntu environemt to build and run IoTivity
    ```
    # Run the following in an Ubuntu terminal
    sudo apt-get install build-essential git scons libtool autoconf valgrind doxygen wget unzip
    sudo apt-get install libboost-dev libboost-program-options-dev libboost-thread-dev uuid-dev libexpat1-dev libglib2.0-dev libsqlite3-dev libcurl4-gnutls-dev
    ```

5. Clone the IoTivity repo
    ```
    # Master
    git clone https://gerrit.iotivity.org/gerrit/iotivity

    # Release branch
    git clone -b 1.3-rel https://gerrit.iotivity.org/gerrit/iotivity
    ```

4. Build IoTivity
    ```
    # Release Build
    scons

    # debug build
    scons RELEASE=0

    # Loogging build
    scons LOGGING=1 LOG_LEVEL=DEBUG
    ```

## Installing the CTT

## Running the CTT against IoTivity
