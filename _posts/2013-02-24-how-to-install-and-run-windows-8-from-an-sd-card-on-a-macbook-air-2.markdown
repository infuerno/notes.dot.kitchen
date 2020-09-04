---
layout: post
title: How to install and run Windows 8 from an SD card on a MacBook Air
date: '2013-02-24 23:15:00'
---


I have a MacBook Air with a 120GB hard drive. I’d partioned off 50GB for a windows install which was happily working with BootCamp, but I was increasingly running out of disk space. I bought a 64GB SD card and spent the best part of the day getting BootCamp to boot this one instead. Here’s how:

1. Download a 64 bit Windows install ISO and make a bootable USB drive using BootCamp Assistant. I used Windows 8 Enterprise N x64.

2. Download and install the trial version of VMWare.

3. Create a new virtual machine, making sure you choose a 64 bit template. Shutdown the machine.

4. Plug in your SD card and check the disk number of both this and your new bootable USB drive via Terminal with

> diskutil list

5. Create .vmdk files for to represent both by running the following, substituting the correct disk number in each case:

> /Applications/VMware\ Fusion.app/Contents/Library/vmware-rawdiskCreator create /dev/disk1 fullDevice ./disk1 lsilogic

6. Copy these two files into the .vmware folder of the virtual machine created earlier (in ~/Documents/Virtual Machines/ by default)

7. Edit the .vmx file in this folder to add the drives specified in these .vmdks to your virtual machine (rather than the one(s) created). I set the first one to be the bootable usb one.

> scsi0:0.present = “TRUE”  
>  scsi0:0.fileName = “disk2.vmdk”  
>  scsi1:0.present = “TRUE”  
>  scsi1:0.fileName = “disk1.vmdk”

8. Force the VM to use EFI by adding the following also to the .vmx file:

> firmware=”efi”

9. Start the virtual machine which should boot off the USB and allow you to install Windows to the SD card.

10. Once the VM is completely installed, shut it down, restart your mac, hold down the option key at boot and all being well you’ll see the EFI drive there and can boot straight into it. You can also boot into it via VMWare or VirtualBox if you’re in the middle of running Mac OS X and

 

 


