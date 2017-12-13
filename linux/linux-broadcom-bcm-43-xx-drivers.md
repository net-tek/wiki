<!-- TITLE: Linux Broadcom Bcm 43 Xx Drivers -->
<!-- SUBTITLE: How to manage Linux Broadcom Bcm 43 Xx Drivers -->

# Introduction
This page provides support information on Broadcom BCM43xx wireless network cards. The aim of Ubuntu is to ensure all card models work automatically with no, or minimal configuration. For example, via **System > Administration > Hardware/Additional Drivers**. If you are having a WiFi issue, please see below on getting this addressed.

# Identifying Your Broadcom BCM43xx Chipset
With this information, you may assess what drivers are supported for your card, and how to switch to a different driver from the instructions below.

## Internal cards

To identify a card that was installed inside your computer prior to purchase, please open a Terminal and execute:

`lspci -vvnn | grep -A 9 Network` 

This will display:

03:00.0 Network controller [0280]: Broadcom Corporation BCM4331 802.11a/b/g/n [14e4:4331] (rev 02)
Subsystem: Apple Inc. AirPort Extreme [106b:00d6]
Control: I/O- Mem+ BusMaster+ SpecCycle- MemWINV- VGASnoop- ParErr- Stepping- SERR- FastB2B- DisINTx-
Status: Cap+ 66MHz- UDF- FastB2B- ParErr- DEVSEL=fast >TAbort- SERR-  Latency: 0, Cache Line Size: 256 bytes
Interrupt: pin A routed to IRQ 17
Region 0: Memory at a0600000 (64-bit, non-prefetchable) [size=16K]
Capabilities: 
Kernel driver in use: wl 

You now know:

* The Chip ID: BCM4331
* The PCI-ID: 14e4:4331
* Kernel driver in use: wl

## USB cards

One will want to execute at a terminal:
usb-devices 
Drivers available in Ubuntu
The following is an overview of the different drivers that are available for Broadcom wireless devices.

Broadcom STA Wireless driver (Proprietary)

For Chip ID BCM 4311, 4312, 4313, 4321, 4322, 4331, 4352, 4360, 43142, 43224, 43225, 43227, 43228, and others as suggested by Broadcom. 

The propietary Broadcom STA Wireless driver is maintained upstream by Broadcom. As this driver is closed source, fixes in the driver itself may only be provided by Broadcom. As a convenience, Ubuntu offers two versions of this driver:
The bcmwl-kernel-source package aims to offer a later version for a given release. Instructions for installation may be found later in this article.
The broadcom-sta package aims to offer an earlier version for a given release. For further installation instructions, please see here.

b43 driver (Open-source)

For Chip ID BCM 4306 (rev 03), 4309, 4311, 4312, 4318, 4322, 4331, 43224 and 43225. 

The b43 infrastructure is composed of two parts. The first is the firmware-b43-installer package. This is simply a script to extract and install the b43 driver firmware, maintained by the Ubuntu community. The second is the b43 driver, maintained upstream by the Linux kernel community. Instructions to install the package may be found below.

b43legacy driver (Open-source)

For Chip ID BCM 4301, 4306 (rev 02), and 4309. 

The b43legacy infrastructure is composed of two parts. The first is the firmware-b43legacy-installer package. This is simply a script to extract and install the b43legacy driver firmware, maintained by the Ubuntu community. The second is the b43 driver, maintained upstream by the Linux kernel community. Instructions to install the package may be found below.

brcmsmac driver (Open-source)

For Chip ID BCM 4313, 43224 and 43225. 

The open-source brcmsmac driver for PCIe devices is available from the brcm80211 module of the linux kernel package, maintained upstream by the linux kernel community. For more granular support information, please see their wiki page here.

brcmfmac driver (Open-source)

SDIO: For Chip ID BCM 4329, 4330, 4334, 4335, 4354, 43143, 43241, and 43362. 
USB: For Chip ID BCM 43143, 43242, 43566, and 43569. 

The open-source brcmfmac driver is available from the brcm80211 module of the linux kernel package, maintained upstream by the linux kernel community. For more granular support information, please see their wiki page here.

rndis_wlan driver (Open-source)

For Chip ID BCM 4320. 

The open-source rndis_wlan driver is available from the linux kernel package, maintained upstream by the linux kernel community. For more granular support information, please see their wiki page here.

ndiswrapper (Open-source)

For all chip IDs. 

The ndiswrapper package utilizes the Windows closed source drivers to activate your WiFi card. It is maintained upstream here. For installation instructions, please see here.

Installing STA drivers
STA - Internet access

If you have some other kind of Internet access on your computer (e.g. via an ethernet cable) then use the instructions below.

12.04 (Precise Pangolin)
Open a Terminal and install the bcmwl-kernel-source package:
sudo apt-get update
sudo apt-get --reinstall install bcmwl-kernel-source
Note: If you see the message "Module build for the currently running kernel was skipped since the kernel source for this kernel does not seem to be installed" then you are missing the appropriate generic linux-header package(s).
To test the driver (and remove the need for a computer restart) use:
sudo modprobe -r b43 ssb wl brcmfmac brcmsmac bcma
sudo modprobe wl
Allow several seconds for the network manager to scan for available networks before attempting a connection. The bcmwl-kernel-source package should automatically blacklist the open source drivers so that the STA driver is the only one in use.
Back to top
STA - No Internet access

If you do not have any other means of Internet access on your computer, you can install the bcmwl-kernel-source package from the restricted folder under ../pool/restricted/b/bcmwl on the Ubuntu install media.
Note: The bcmwl-kernel-source package depends on the linux-headers packages so you may need to first retrieve the appropriate package(s) from the online repositories. A running LiveCD/LiveUSB environment has these packages (allowing the wireless to work), but an installed system may not. Make sure you have the linux-headers package that matches your current kernel version, plus the appropriate generic header packages so that they are automatically updated on a kernel upgrade. To find out your current kernel use the command:
uname -r
To find what linux-headers packages you have installed use the command:
dpkg --get-selections | grep headers
Systems installed from CDROM can add the install CD as a package source and install bcmwl-kernel-source using apt-get as above. However, if you want to do it manually then the instructions are as follows:
Navigate the install media and install the packages listed below by double clicking OR install the packages consecutively from a Terminal (in the commands below the install media is mounted at /cdrom, but yours maybe different): 
../pool/main/d/dkms
cd /cdrom/pool/main/d/dkms
<span id="line-2-3" class="anchor"></span>sudo dpkg -i dkms*
../pool/main/p/patch
cd /cdrom/pool/main/p/patch
<span id="line-2-4" class="anchor"></span>sudo dpkg -i patch*
../pool/main/f/fakeroot
cd /cdrom/pool/main/f/fakeroot
<span id="line-2-5" class="anchor"></span>sudo dpkg -i fakeroot*
../pool/restricted/b/bcmwl
cd /cdrom/pool/restricted/b/bcmwl
<span id="line-2-6" class="anchor"></span>sudo dpkg -i bcmwl-kernel-source*
Back to top
Upstream 802.11 Linux STA driver

For download and install instructions, please see http://www.broadcom.com/support/802.11/linux_sta.php .
Back to top

Installing b43/b43legacy firmware
The Ubuntu kernel now provides the b43 driver, however due to copyright restrictions not the proprietary firmware which is required to run your card. The following instructions explain how to extract the required firmware.

b43 - Internet access

12.04 (Precise Pangolin) - 14.04 (Trusty Tahr)
Open a Terminal and if you haven't already done so, update your package list:
sudo apt-get update
If you have a b43 card use the command
sudo apt-get install firmware-b43-installer
or, if you need the b43legacy driver, use:
sudo apt-get install firmware-b43legacy-installer
or, (12.04) if you need a LP-PHY version (e.g BCM4312), use:
sudo apt-get install firmware-b43-lpphy-installer
Restart the computer or reload the b43/b43legacy module as outlined in the Switching between drivers section below (replace b43 with b43legacy where appropriate).
Back to top
b43 - No Internet access

If you do not have any other means of Internet access from Ubuntu, then you will have to download the firmware from another computer with Internet access, from an existing OS on another partition, or before you install Ubuntu. You will also need the b43-fwcutter package which is usually included on the install media or can be downloaded from the official online repositories.
Install the b43-fwcutter package. This is usually located on the Ubuntu install media under /cdrom/pool/main/b/b43-fwcutter/ or you can download the binary '.deb' package by following the links on launchpad.
Double click on the package to install or in a Terminal issue the following commands:
<span id="line-1-15" class="anchor"></span>cd /cdrom/pool/main/b/b43-fwcutter/
<span id="line-2-7" class="anchor"></span>sudo dpkg -i b43-fwcutter* 
On a computer with Internet access, download the required firmware file: 
b43legacy - http://downloads.openwrt.org/sources/wl_apsta-3.130.20.0.o 
b43 (12.04 Precise Pangolin) - http://mirror2.openwrt.org/sources/broadcom-wl-5.10.56.27.3_mipsel.tar.bz2 
b43 (14.04 Trusty Tahr) - http://www.lwfinger.com/b43-firmware/broadcom-wl-5.100.138.tar.bz2 
For the latest information on what files to download see http://wireless.kernel.org/en/users/Drivers/b43#Other_distributions_not_mentioned_above and http://wireless.kernel.org/en/users/Drivers/b43/developers .
Copy the downloaded file to your home folder. Open a new Terminal and use b43-fwcutter to extract and install the firmware: 
b43legacy
<span id="line-1-16" class="anchor"></span>sudo b43-fwcutter -w /lib/firmware wl_apsta-3.130.20.0.o
b43 (12.04 Precise Pangolin)
<span id="line-1-17" class="anchor"></span>tar xfvj broadcom-wl-5.10.56.27.3_mipsel.tar.bz2
<span id="line-2-8" class="anchor"></span>sudo b43-fwcutter -w /lib/firmware broadcom-wl-5.10.56.27.3/driver/wl_apsta/wl_prebuilt.o
b43 (14.04 Trusty Tahr)
<span id="line-1-18" class="anchor"></span>tar xfvj broadcom-wl-5.100.138.tar.bz2
<span id="line-2-9" class="anchor"></span>sudo b43-fwcutter -w /lib/firmware broadcom-wl-5.100.138/linux/wl_apsta.o
Restart the computer or reload the b43/b43legacy module as outlined in the Switching between drivers section below (replace b43 with b43legacy where appropriate).

Back to top

Switching between drivers
If you card is supported by more than one driver then use the modprobe command to test the drivers. First unload all conflicting drivers (this includes removing the driver you're trying to install):

<span id="line-1-19" class="anchor"></span>sudo modprobe -r b43 bcma
<span id="line-2-10" class="anchor"></span>sudo modprobe -r brcmsmac bcma
<span id="line-3-1" class="anchor"></span>sudo modprobe -r wl
To load a specific driver use one of the following commands:
<span id="line-1-20" class="anchor"></span>sudo modprobe b43
<span id="line-2-11" class="anchor"></span>sudo modprobe brcmsmac
<span id="line-3-2" class="anchor"></span>sudo modprobe wl
Allow several seconds for the network manager to scan for available networks before attempting a connection.
After a reboot the system may auto-load a different driver to the one you wanted to use. Consequently, for permanent use, you may find it necessary to blacklist the driver/module you are not using. In the command below replace drivername with the driver you want to blacklist:
<span id="line-1-21" class="anchor"></span>echo "blacklist drivername" | sudo tee -a /etc/modprobe.d/blacklist-broadcom-wireless.conf
Update the initramfs after any changes to the blacklist files:
<span id="line-1-22" class="anchor"></span>sudo update-initramfs -u
Note: The bcmwl-kernel-source package will automatically blacklist the open source drivers/modules in /etc/modprobe.d/blacklist-bcm43.conf.
If you wish to permanently use the open source drivers then remove the bcmwl-kernel-source package:
<span id="line-1-23" class="anchor"></span>sudo apt-get purge bcmwl-kernel-source
Ensure that the driver/modules you wish to use are not blacklisted in any of the other files in /etc/modprobe.d .
Back to top

Unsupported devices
If your wifi card/chipset and/or various modes are not supported by the STA driver or the open source kernel drivers, then you will need to go for ndiswrapper - this will allow you to use the Windows closed source drivers to activate your wifi card.
Back to top

Known Issues
LP#1010931 14e4:4727 [Dell Vostro 3555] Broadcom BCM4313 5GHz doesn't work but 2.4GHz does
The root cause is the card only transmits/receives on the single-band 2.4GHz only, so it would never broadcast at 5GHz.
Back to top

Filing bug reports
Broadcom STA Wireless driver

Before filing a bug report against either the bcmwl-kernel-source or broadcom-sta-source package, one from the Ubuntu repositories is installed (not a recompiled/custom version) and then execute one of the following via a terminal:
<span id="line-1-24" class="anchor"></span>ubuntu-bug bcmwl-kernel-source
<span id="line-2-12" class="anchor"></span>ubuntu-bug broadcom-sta-source 
If this doesn't work, or doesn't include all of the following information, please ensure all of the below is provided:
Please include only one (not both) of the following corresponding to which driver series you are filing a report against:
<span id="line-1-25" class="anchor"></span>apt-cache policy bcmwl-kernel-source 
or:
<span id="line-1-26" class="anchor"></span>apt-cache policy broadcom-sta-source 
Please execute the following via a terminal and post the results in your report:
<span id="line-1-27" class="anchor"></span>lspci -vvnn | grep -A 9 Network
<span id="line-2-13" class="anchor"></span>lsb_release -rd
<span id="line-3-3" class="anchor"></span>uname -a
<span id="line-4-1" class="anchor"></span>sudo dmidecode -s bios-version
<span id="line-5-1" class="anchor"></span>sudo dmidecode -s bios-release-date 
The full manufacturer and model of your computer as noted on the sticker of the computer itself.
Did this problem not occur in a previous release? If so, which one(s) specifically?
Does this problem occur with the latest version of Ubuntu?
If available, please comment to how testing the relevant open source driver for your card type provides a WORKAROUND. If your chipset is supported as per above, but doesn't work, please file a bug following the b43 driver procedure below.
Please provide the router manufacturer, model, and firmware version.
Please comment to how testing ndiswrapper for your card type provides a WORKAROUND. If it doesn't work, please file a bug report as per the support article.

If the version of the driver you are using in the repository is the latest version available as per Broadcom, it is also advised to send them an e-mail via their contact page, and post their response to your report.
If the version of the driver you are using in the repository is an older version than that available from Broadcom, then contacting them would not apply. Instead, an investigation would need to occur to see if the version available for your release should be changed.

b43/b43legacy firmware utility

Before filing a bug report about b43 or b43legacy, it's important to distinguish this as a issue with the firmware extraction script, or the driver itself. If it's an issue with the script, one will need to install the version from the Ubuntu repositories (not a recompiled/custom version) and then execute via a terminal either:
<span id="line-1-28" class="anchor"></span>ubuntu-bug firmware-b43-installer
or:
<span id="line-1-29" class="anchor"></span>ubuntu-bug firmware-b43legacy-installer
b43/b43legacy driver

For bugs regarding the b43 or b43legacy driver, please execute the following via a terminal:
<span id="line-1-30" class="anchor"></span>ubuntu-bug linux
Also, please provide the following:
The full manufacturer and model of your computer as noted on the sticker of the computer itself.
Did this problem not occur in a previous release? If so, which one(s) specifically?
Does this problem occur with the latest version of Ubuntu?
If available, please comment to how testing test the relevant Broadcom STA Wireless driver for your card type provides a WORKAROUND. If it doesn't, please file a report as per the procedure above.
Please provide the router manufacturer, model, and firmware version.
Please comment to how testing ndiswrapper for your card type provides a WORKAROUND. If it doesn't work, please file a bug report as per the support article.

Once all of the required information is present, if the version of the driver you are using is the latest version available from the Ubuntu repositories, then one would want to e-mail the b43-dev mailing list following this procedure.