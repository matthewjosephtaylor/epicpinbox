# Pinbox backend server install

## Raspberry Pi 4

### Prerequisites

- Raspberry Pi Imager: https://www.raspberrypi.org/downloads/
- Ubuntu: https://ubuntu.com/download/raspberry-pi
  - 20.04.1 LTS 64-bit for Raspberry Pi 4

## Install OS

- Use Imager to flash sd card
- Install flash card in RP, add power and go :)

## Misc

- check CPU freq `find /sys/devices/system/cpu/cpu[0-3]/cpufreq/cpuinfo_cur_freq -type f | xargs cat`
- check Temp `cat /sys/class/thermal/thermal_zone*/temp`
- voltage?

## Links

- https://wiki.ubuntu.com/ARM/RaspberryPi

## Ubuntu OS Setup

- Docker from https://docs.docker.com/engine/install/ubuntu/

### Hostname

```
hostnamectl set-hostname <my-hostname>
```

### mDNS (avahi)

```
apt-get install avahi-daemon
```

### Docker

```
apt-get install \
 apt-transport-https \
 ca-certificates \
 curl \
 gnupg-agent \
 software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

add-apt-repository \
 "deb [arch=arm64] https://download.docker.com/linux/ubuntu \
 $(lsb_release -cs) \
 stable"

apt-get update

apt-get install docker-ce docker-ce-cli containerd.io

docker run hello-world
```

### ZFS

```
apt-get install zfsutils-linux
zpool create -m none tank /dev/disk/by-id/usb-...
zfs set compression=lz4 atime=off tank
zfs create -o mountpoint=/pinbox tank/pinbox
```

### Pinbox User Setup

```
adduser pinbox
usermod -aG docker pinbox
chown pinbox:pinbox /pinbox/
```

### IPFS

```
sudo su - pinbox
cd /pinbox
git clone https://github.com/matthewjosephtaylor/easy_ipfs.git
cd easy_ipfs
./ipfs-build
./ipfs-init
./ipfs-start -ep

# Optional depending on setup (will require a container restart)
./ipfs-enable-api
./ipfs-enable-cors
./ipfs-enable-gateway
./ipfs-enable-unset-bootstrap
./ipfs-set-storage-size 3.5TB

```
