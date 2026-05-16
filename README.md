# How to install Home Assistant on Debian 14 "Forky" with QEMU/KVM virtual image

It's been a tough learning path for me, so I decided to create this tutorial and spare you all that effort.<br>
In this tutorial I will use an old Intel x86 PC as a Home Assistant server.<br>
Please note: in this scenario server is connected with ethernet cable to the LAN router. Wifi has not been tested.

## Table of contents
1. [Install Debian 14 "Forky"](https://rc-pl.github.io/home-assistant-installation-on-debian-14/#1-install-debian-14-forky)
2. [Set up bridge networking on your host OS](https://rc-pl.github.io/home-assistant-installation-on-debian-14/#2-set-up-bridge-networking-on-your-host-os)
3. Install QEMU/KVM and set it up
4. Deploy and run Home Assistant Operating System on QEMU

## 1. Install Debian 14 "Forky"
### Download Debian 14 NETINST image and save it on your PC

We're going to install Debian minimal server, therefore NETINST installer is sufficient for this task. As of may 2026 Debian 14 "Forky" is still in testing phase, so use this URL [https://cdimage.debian.org/cdimage/daily-builds/daily/arch-latest/amd64/iso-cd/](https://cdimage.debian.org/cdimage/daily-builds/daily/arch-latest/amd64/iso-cd/){:target="_blank" rel="noopener"} to download the [debian-testing-amd64-netinst.iso](https://cdimage.debian.org/cdimage/daily-builds/daily/arch-latest/amd64/iso-cd/debian-testing-amd64-netinst.iso){:target="_blank" rel="noopener"} ISO.

As soon as Debian 14 "Forky" becomes stable (expected to happen in 2027), use this URL to download NETINST installer ISO [https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/){:target="_blank" rel="noopener"}.

### Write Debian ISO to USB disk with Rufus

Go to [https://rufus.ie/](https://rufus.ie){:target="_blank" rel="noopener"} and download Portable version of this program.

Plug USB disk into your PC and start Rufus. Make sure USB disk is shown in Device section. Select Debian ISO and click Start to write it on the USB disk.
<p align="center">
  <img src="./media/1-1.png" alt="Rufus settings to write Debian ISO on USB disk" width="70%">
</p>
Rufus may ask you some questions before writing the image. Just click "OK" / "Confirm" to start the process.

### Install Debian on the server machine

Connect your server with RJ45 cable to the LAN router.

Put Debian installer USB disk in USB port.

Turn on the server and enter the BIOS by pressing relevant key on the keyboard. It might be something like DEL, F1, F12 - depends on the manufacturer.

In BIOS make sure that USB disk is selected as a first boot device. Make sure that virtualization is possible and is enabled for your processor.

Save all changes and restart the server. It should now boot from the USB disk and you will see a welcome screen for Debian installer.

Installation is pretty straightfoward. Just follow below video. **STOP** before GNOME installation (we don't need that).
<p align="center">
  <a href="https://www.youtube.com/watch?v=w2HFAhIIyag" target="_blank">
    <img src="./media/1-2.png" alt="Debian installation video" width="50%">
  </a>
</p>

After installation is finished remove USB disk and reboot the machine.

Now your Debian 14 server is up and running and can be used in both ways:<br>
- host OS on which you will run virtual server with Home Assistant Operating System<br>
- general purpose Linux server where you can run any additional services (pihole, file server, whatever)


## 2. Set up bridge networking on your host OS
By default Debian configures your network interface as a DHCP client, so you get a dynamic IP from your LAN router.<br>
We want to have a static IP on the server, so it can always be connected with the same IP.

Let's first install **bridge-utils** package
```bash
apt install bridge-utils
```

Now check your current IP
```bash
ip a
```

<p align="center">
    <img src="./media/2-1.png" alt="Check IP in Debian" width="100%">
</p>

In this example we see two network interfaces: **lo** virtual loopback interface and **enp1s0** physical network card connected to LAN router with ethernet cable.<br>
Physical network interface received 192.168.88.16 IP via DHCP.<br>
Now it's time to plan what static IP we are going to use. It should be outside DHCP IP range. If you don't know it, safe bet would be to use a high number. In this example I will use **192.168.88.100**.

Let's edit interfaces configuration file
```bash
nano /etc/network/interfaces
```

This is a default view. Your interface name can be different.

<p align="center">
    <img src="./media/2-2.png" alt="Default interfaces in Debian" width="100%">
</p>

Let's define a **br0** bridge and bind a physical interface **enp1s0** as a bridge port.
```bash
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# Set up interfaces manually, avoiding conflicts with, e.g., network manager
iface enp1s0 inet manual

# The primary network interface
auto br0
iface br0 inet static
 address 192.168.88.100
 broadcast 192.168.88.255
 netmask 255.255.255.0
 gateway 192.168.88.1
 bridge_ports enp1s0
 bridge_stp off
 bridge_waitport 0
 bridge fd 0
```

Let's reboot the server.
```bash
systemctl reboot
```

Check current network status.
```bash
ip a
```

<p align="center">
    <img src="./media/2-3.png" alt="Network bridge in Debian" width="100%">
</p>

Bridge **br0** is up and static IP is assigned.

## 3. Install QEMU/KVM and set it up
Home Assistant Operating System (HAOS) can be deployed in couple different ways.<br>
It can be installed standalone on the server, but this way we get a very restriced Linux system without possibility of installing and running additional services.<br>
Therefore in this tutorial HAOS will be run as a virtual server guest in QEMU and our host server (Debian) will provide full functionality of a Linux server for running other services.
