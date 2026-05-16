# How to install Home Assistant on Debian 14 "Forky" with QEMU/KVM virtual image

It's been a tough learning path for me, so I decided to create this tutorial and spare you this effort.

## Table of contents
1. [Install Debian 14 "Forky"](https://rc-pl.github.io/home-assistant-installation-on-debian-14/#install-debian-14-forky)
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
