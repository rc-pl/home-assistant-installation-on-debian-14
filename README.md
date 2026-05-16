# How to install Home Assistant on Debian 14 "Forky" with QEMU/KVM virtual image

It's been a tough learning path for me, so I decided to create this tutorial and spare you this effort.

## Table of contents
1. [Install Debian 14 "Forky"](https://rc-pl.github.io/home-assistant-installation-on-debian-14/#install-debian-14-forky)
2. Set up bridge networking on your host OS
3. Install QEMU/KVM and set it up
4. Deploy and run Home Assistant Operating System on QEMU

### Install Debian 14 "Forky"
1. Download Debian 14 NETINST image and write it on USB disk with Rufus

We're going to install Debian minimal server, therefore NETINST installer is sufficient for this task, as it provides only necessary set of packages.<Br>
As of may 2026 Debian 14 "Forky" is still in testing phase, so use this URL
[https://cdimage.debian.org/cdimage/daily-builds/daily/arch-latest/amd64/iso-cd/](https://cdimage.debian.org/cdimage/daily-builds/daily/arch-latest/amd64/iso-cd/)
to download the [debian-testing-amd64-netinst.iso](https://cdimage.debian.org/cdimage/daily-builds/daily/arch-latest/amd64/iso-cd/debian-testing-amd64-netinst.iso) ISO

3. 
