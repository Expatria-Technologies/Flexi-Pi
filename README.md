## Flexi Pi

Flexi-Pi is a custom configured LinuxCNC image for use with the [Flexi_HAL](https://github.com/Expatria-Technologies/Flexi-HAL) CNC controller and Remora firmware.The Flexi LinuxCNC components are pre-installed along with all of their dependencies, and reference configurations are included to get you started. **You will need to edit these configurations for your specific machine before attempting motion.** The base configuration runs QtDragon_hd as the default supported UI. 

Both the Raspberry Pi 5 and Raspberry Pi 4 are currently supported. Performance is significantly better with a Pi 5, however the Pi 4 image here performs better than the previous releases. 

**To get started:**
* Download the release for your specific Pi board (Pi 4 or Pi 5)
* Extract the image archive
* Flash the image to an an 8GB or larger SD card using dd, balenaEtcher, or the tool of your choice. The filesystem will be automatically resized on first boot to fill the entirety of the SD card.
* Install the Pi onto the Flexi-HAL headers and insert the SD card into the Pi.
* Ensure the jumpers are set as shown in the Flexi-HAL Configuration section of this README
* Connect your display, keyboard, etc. to the Pi
* Connect power to the Pi/Flexi-HAL and boot the OS
* Read the contents of the README present on the desktop for instructions specific to the image used. This includes some hints on flashing your Flexi-HAL with the Remora firmware, and starting LinuxCNC.

The default username and password are both `expatria`.

A high-endurance SD card from a reputable manufacturer is recommended. The Sandisk U3 High Endurance cards have performed well in our testing. Select a card with a high read/write speed for the best performance. 


Also pre-installed in this image are the standard Remora-spi and Remora-eth-3.0 components. These are included on an as-is basis, but are updated with each image build. 


## Flexi-HAL Configuration
Jumpers need to be in place in the marked locations on the Flexi-HAL for the Pi to communicate with the MCU for firmware flashing:

<img src="https://github.com/Expatria-Technologies/remora-flexi-hal/raw/master/Images/Jumper_locations.png" width="500">

These are shipped in place by default from the Expatria shop, but if they have been moved or removed they will need to be replaced. 
 

### Support

Support is provided primarily through our Discord Server: https://discord.gg/bSEXy7GhcZ
If you need help getting started or with configuration, please feel free to reach out there. 

<br></br>
<br></br>


---  

## Building on Ubuntu 24.04:


### Currently supported boards

* **Raspberry Pi 5** bcm2712 / ARM64

* **Raspberry Pi 4/400** bcm2711 / ARM64


#### Install dependencies

  

```sh

make ccompile # Install x86-64 dependencies

make ncompile # Install Aarch64 dependencies

```

  

#### Menu interface

  

```sh

make config # Create user data file

make menu # Open menu interface

make dialogrc # Set builder theme (optional)

```

  

#### Command list

  

```sh

make list # List boards

make all board=xxx # Kernel > rootfs > image

make kernel board=xxx # Builds linux kernel package

make commit board=xxx # Builds linux kernel package

make rootfs board=xxx # Create rootfs tarball

make image board=xxx # Make bootable image

```

  

#### Miscellaneous

  

```sh

make clean # Clean up rootfs and image errors

make purge # Remove source directory

make purge-all # Remove source and output directory

make commands # List more commands

make check # Shows latest revision of selected branch

```

  

#### Config Menu

* Review the userdata.txt file for further options: locales, timezone, nameserver(s) and extra wireless support

* 1 active | 0 inactive

```sh

Name: # Your name

Username: # Your username

Password: # Your password

Enable root: # Set root password to `toor`

  

Linux kernel

Branch: # Supported: 6.1.y and above

Build: # Kernel build version number

Menuconfig: # Kernel menuconfig

Compiler: # GNU Compiler Collection / Clang

Ccache: # Compiler cache

  

Distribution

Distro: # Supported: debian, devuan and ubuntu

Release: # Debian: bullseye, bookworm, testing, unstable and sid

# Devuan: chimaera and daedalus (broken: excalibur, testing, unstable, ceres)

# https://www.devuan.org/os/announce/excalibur-usrmerge-announce-2024-02-20.html

# Ubuntu: focal, jammy and noble

NetworkManager # 1 networkmanager | 0 ifupdown

  

Customize

Defconfig: # User defconfig

Name: # Name of _defconfig (Must be placed in defconfig dir.)

  

User options

Verbosity: # Verbose

Devel Rootfs: # Developer rootfs tarball

Compress img: # Auto compress img > img.xz

User scripts: # Review the README in the files/userscripts directory

User service: # Create user during first boot (bypass the user information above)

```

  

#### Customize image

* custom.txt

```sh

# Image Size

IMGSIZE="4096MB"

  

# Root Filesystem Types: ext4 btrfs xfs

FSTYPE="ext4"

  

# Shrink Image

SHRINK="true"

  

# Hostname

HOSTNAME="raspberrypi"

  

# Branding: true false

BRANDING="false"

MOTD="Raspberry Pi"

```

  

#### User defconfig

```sh

# Config placement: defconfig/$NAME_defconfig

The config menu will append _defconfig to the end of the name

in the userdata.txt file.

```

  

#### User patches

  

```sh

Patches "-p1" placed in userpatches are applied during compilation.

```

  

#### Preferred commit

```sh

# Example

ENABLE_COMMIT="1"

COMMIT="9ed4f05ba2e2bcd9065831674e97b2b1283e866d"

```

  

### Usage

* Review the [Wiki](https://github.com/pyavitz/rpi-img-builder/wiki/Options-&-Scripts)

* The boot partition is labelled BOOT

#### BOOT: useraccount.txt

* Headless: ENABLE="true" and fill in the variables (recommended)

* Headful: ENABLE="false" and get prompted to create a user account

```sh

ENABLE="false" # Set to true to enable service

NAME="" # Your name

USERNAME="" # Username

PASSWORD="" # Password

```

  

#### BOOT: credentials.txt

```sh

Set to ENABLE="true" and input your wifi information.

ENABLE="false" # Enable service

  

SSID="" # Service set identifier

PASSKEY="" # Wifi password

COUNTRYCODE="" # Your country code

  

# set static ip (ifupdown)

MANUAL="false" # Set to true to enable a static ip

IPADDR="" # Static ip address

NETMASK="" # Your Netmask

GATEWAY="" # Your Gateway

NAMESERVERS="" # Your preferred dns

  

# set static ip (network-manager)

MANUAL="false" # Set to true to enable a static ip

IPADDR="" # Static ip address

GATEWAY="" # Your Gateway

DNS="" # Your preferred dns

  

# change hostname

HOSTNAME="raspberrypi" # Hostname

  

For headless use: ssh user@ipaddress

```

#### System Menu: `menu-config`

<img src="https://i.imgur.com/vwFVBzF.png" alt="Main Menu" />

  

---