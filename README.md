# How to install Home Assistant on Debian 14 "Forky" with QEMU/KVM virtual image

It's been a tough learning path for me, so I decided to create this tutorial and spare you all that effort.<br>
In this tutorial I will use an old Intel x86 PC as a Home Assistant server.<br>
Please note: in this scenario server is connected with ethernet cable to the LAN router. Wifi has not been tested.

## Table of contents
1. [Install Debian 14 "Forky"](https://rc-pl.github.io/home-assistant-installation-on-debian-14/#1-install-debian-14-forky)
2. Set up bridge networking on your host OS
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
    <img src="./media/1-1.png" alt="Rufus settings to write Debian ISO on USB disk" width="50%">
  </a>
</p>

