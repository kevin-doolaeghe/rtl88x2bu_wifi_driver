# TP-Link Archer T3U driver for Linux

## Sources

* [Git Repository - Realtek Network Adapter Card Driver](https://github.com/cilynx/rtl88x2bu)
* [Installation Example](https://forums.linuxmint.com/viewtopic.php?t=306556)
* [Official TP-Link Installation Guide](https://static.tp-link.com/2018/201805/20180508/Installation%20Guide%20for%20Linux.pdf)
* [Linux Kernel - Modules Tutorial](https://doc.ubuntu-fr.org/tutoriel/tout_savoir_sur_les_modules_linux)
* [rtl8188eus driver with monitor mode and packet injection](https://github.com/aircrack-ng/rtl8188eus)

## Installation (cilynx/rtl88x2bu repository)

* Install required packages :
```
sudo apt install git dkms rsync build-essential bc
```

* Clone the driver repository :
```
git clone https://github.com/cilynx/rtl88x2bu.git
cd rtl88x2bu
```

* Install the driver :
```
VER=$(sed -n 's/\PACKAGE_VERSION="\(.*\)"/\1/p' dkms.conf)
sudo rsync -rvhP ./ /usr/src/rtl88x2bu-${VER}
sudo dkms add -m rtl88x2bu -v ${VER}
sudo dkms build -m rtl88x2bu -v ${VER}
sudo dkms install -m rtl88x2bu -v ${VER}
sudo modprobe 88x2bu
```

Note : On a Raspberry Pi board, you must specify `ARCH=arm64` :
```
make ARCH=arm64 && sudo make install
sudo modprobe 88x2bu
```

* Check if the wireless adapter is correctly recognized :
```
iwconfig
```
Note : The `RTL8812BU chipset **does not support monitor mode**.

## Remove a driver

* Display driver list :
```
lsmod
```

* Remove 88x2bu driver :
```
sudo modprobe -r 88x2bu
```

## Installation (aircrack-ng/rtl8188eus repository)

* Clone the driver repository :
```
git clone https://github.com/aircrack-ng/rtl8188eus.git
cd rtl8188eus
```

* Install the driver :
```
make
sudo make install
sudo modprobe 8188eu
```

## `aircrack-ng` - packet injection test

* Install `aircrack-ng` package :
```
sudo apt install aircrack-ng
```

* Display wireless adapters :
```
sudo airmon-ng
```

* Kill services that uses wireless adapters :
```
sudo airmon-ng check kill
```

* Start monitor mode for `wlan0` adapter :
```
sudo airmon-ng start wlan0
```

* Send 4 deathentication packets :
```
aireplay-ng --deauth 4 -a XX:XX:XX:XX:XX:XX -c XX:XX:XX:XX:XX:XX wlan0
```
