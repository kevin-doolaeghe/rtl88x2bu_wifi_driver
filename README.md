# TP-Link Archer T3U driver for Linux

## Sources

* [Git Repository - Realtek Network Adapter Card Driver](https://github.com/cilynx/rtl88x2bu)
* [Installation Example](https://forums.linuxmint.com/viewtopic.php?t=306556)
* [Official TP-Link Installation Guide](https://static.tp-link.com/2018/201805/20180508/Installation%20Guide%20for%20Linux.pdf)

## Installation

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

* Check if the wireless adapter is correctly recognized :
```
iwconfig
```
