## Configuring an Ubuntu VM

1. Create an Ubuntu VM: Recommended virtual disk size: minimum of 20G

2. Install support tools
    ```
    sudo apt-get install build-essential git scons libtool autoconf valgrind doxygen wget unzip

    sudo apt-get install libboost-dev libboost-program-options-dev libboost-thread-dev uuid-dev libexpat1-dev libglib2.0-dev libsqlite3-dev libcurl4-gnutls-dev
    ```

3. Clone the repo
    ```
    git clone https://gerrit.iotivity.org/gerrit/iotivity # Master
    git clone -b 1.3-rel https://gerrit.iotivity.org/gerrit/iotivity # Rel branch
    ```

4. Build
    ```
    scons
    scons RELEASE=0 // debug build
    scons LOGGING=1 LOG_LEVEL=DEBUG RELEASE=0 // debug build w logging
    ```

## Installing the CTT

## Running the CTT against IoTivity
