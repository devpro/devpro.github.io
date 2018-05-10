# Hyper-V

[Home](../readme.md) > [Windows](./readme.md) > [Hyper-V](./hyper_v.md)

## Readings

- [How to Setup and Use Hyper-V in Windows 10 for OS Virtualization](https://www.tenforums.com/tutorials/2087-hyper-v-virtualization-setup-use-windows-10-a.html#Part4)
- [How to Install CentOS Linux on Hyper-V Virtual Machine in Windows 10](https://www.tenforums.com/tutorials/2291-hyper-v-vm-install-centos-linux-windows-10-a.html)

## Usecases

### Install Ubuntu

- [Changing Ubuntu Screen Resolution in a Hyper-V VM](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/)
- [Supported Ubuntu virtual machines on Hyper-V](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/supported-ubuntu-virtual-machines-on-hyper-v)

```bash
sudo apt-get update
sudo apt-get install linux-virtual-lts-xenial
```

### Install CentOS

```bash
sudo grubby --update-kernel=ALL --args="video=hyperv_fb:1920x1080"
sudo reboot
yum check-update
yum install git
```

References:

- [Hyper-V: How to Change Screen Resolution in CentOS / Red Hat Virtual Machines](https://www.netometer.com/blog/?p=1663)
- [Running VS Code on Linux](https://code.visualstudio.com/docs/setup/linux)
- [Get Docker CE for CentOS](https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-convenience-script)
