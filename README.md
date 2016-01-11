# Hackintosh Instructions

Instructions to install OS X El Capitan (10.11.1) onto my Intel NUC 54250 WYKH

![Intel NUC 54250 WYKH](http://www.jakartanotebook.com/upload/images/54250-ov.jpg)

**Disclaimer**: These instructions are solely for the purpose of academic
learnings. Be warned that anyone following them will be violating the OS X EULA
and is maybe illegal.

### Hardware Specifications

##### Base
See http://www.intel.com/content/www/us/en/nuc/nuc-kit-d54250wykh.html

##### SSD
Crucial 2.5" MX100 256GB SSD (CT256MX100SSD1)

##### Memory
G.SKILL 16GB (2 x 8G) 204-Pin DDR3 SO-DIMM DDR3 1600 (PC3 12800) Laptop Memory (Model F3-1600C11D-16GSL)

##### Wifi/Bluetooth
AzureWave AW-CE123H / 802.11ac/n/b/g + Bluetooth 4.0 / Half-Size PCI-Express MiniCard

##### USB Flash Drive

SanDisk Ultra Fit™ CZ43 64GB USB 3.0 (SDCZ43-064G-G46)

### What's Working

1. 802.11ac at 2.4 and 5GHz
1. Bluetooth (Apple Wireless Keyboard, Magic Trackpad)
1. Audio (2.5" headphone and Cinema Display via USB - non-Thunderbolt)
1. OS X iCloud
1. OS X Handoff

### What's Not Working (Yet)

1. iMessage
   * If you do know what steps are necessary to get iMessage working, please create a issue! I will update this guide.

### References

#### Documentation

1. <a name="unibeast" />[UniBeast with El Capitan](http://www.tonymacx86.com/yosemite-desktop-guides/143976-unibeast-install-os-x-yosemite-any-supported-intel-based-pc.html)
1. [Wifi+Bluetooth](http://www.tonymacx86.com/network/104850-guide-airport-pcie-half-mini-v2.html)

#### Downloads

1. <a name="unibeast" />[UniBeast v6.1.1](http://www.tonymacx86.com/downloads.php?do=cat&id=3)
1. <a name="multibeast" />[MultiBeast v8.0.1](http://www.tonymacx86.com/downloads.php?do=cat&id=3)
1. <a name="efi-mounter" />[EFI Mounter v2](http://www.tonymacx86.com/downloads.php?do=cat&id=10)
1. <a name="clover-configurator" />[Clover Configurator v4.25.0](http://www.tonymacx86.com/downloads.php?do=cat&id=10)
1. <a name="easykext" />[Easykext Utility v2.0](https://insanelydeepak.wordpress.com/2015/04/23/kext-installer-mac-os-x-hackintosh/)
1. <a name="fake-pci-id" />[Fake PCI ID via RehabMan version 2015-1229](https://github.com/RehabMan/OS-X-Fake-PCI-ID)
1. <a name="brcm-patch" />[Broadcom Bluetooth USB devices patch](https://github.com/the-darkvoid/BrcmPatchRAM)
1. <a name="5-ghz-patch" />[Wifi 5GHz support](https://github.com/toleda/wireless_half-mini/blob/master/wireless_bcm94352-110-v4.0.command.zip)
1. <a name="disk-sensei" />[Disk Sensei v1.2](https://www.cindori.org/software/disksensei/)

### TL;DR Instructions

1. UniBeast on USB
1. MultiBeast on SSD
1. Get Wifi 2.4GHz working
1. Get Wifi 5GHz working
1. Get Bluetooth 4LE working
1. Enable TRIM on SSD

### Detail Instructions

#### 1. UniBeast USB Flash Drive

1. Mostly following [UniBeast Reference](#user-content-unibeast) directions. But the **bolds** are worth noting or different.
1. Download OS X El Capitan from App Store on a mac.
1. Format USB flash drive, using **GUID Parititon Table**, OS X Journaled FS. Name the volume "USB".
1. Install UniBeast onto the USB drive.
   * No need for "Legacy USB Support".
   * No need for "Laptop Support".
   * Do **NOT** choose any "Intel HD 3000" graphics card options. Leave off all selections.
     * The NUC has Intel HD 5000. Otherwise you may run into an infinite loop of reboot and never see the OS X installation welcome gui.
1. Copy the following onto your mounted USB volume when UniBeast is finished:
   * [RehabMan-FakePCIID-2015-1229](#user-content-fake-pci-id)
   * [EFI Mounter v2](#user-content-efi-mounter)
   * [Easykext Utility](#user-content-easykext)
   * [MultiBeast](#user-content-multibeast)
   * (optional) [Clover Configurator](#user-content-clover-configurator)

#### 1. OS X Installation

1. Connect USB keyboard and mouse.
1. Hit F10 when NUC boot screen is shown. Choose your USB.
1. You should now be able to boot into the OS X installation wizard via the USB flash drive.
1. Use Disk Utility to format the SSD drive to OS X Extended Journaled. Use 1 Partition, and name it "Macintosh HD".
1. Continue to set it up. Wifi won't be working until later. Skip any Wifi or iCloud set up.
1. When done, drag all files (minus the "Install El Capital.app") from your USB drive mount onto the Desktop.

#### 2. MultiBeast onto SSD

1. Under Quick Start: Select UEFI Boot Mode.
1. Under Drivers/Audio: Select ALX283 Brix Pro and NUC. This enables sound.
1. Under Customize/SSDT Options: Select Sandy Bridge Core i5. (Not sure what this does, but this NUC is an i5).
1. Configuration should look like this: [Screenshot](http://docs.google.com/gview?embedded=true&url=https://github.com/stephenchu/hackintosh/raw/init/static/MultiBeast_Summary.pdf)

Unplug USB drive and restart OS X.

#### 3. Enable Wifi

1. Unzip the [RehabMan-FakePCIID-2015-1229](#user-content-fake-pci-id) zip file by running the following in Terminal.app:

   ```unzip -d ~/Desktop/FakePCIID ~/Desktop/RehabMan-FakePCIID-2015-1229.zip```

   **Note**: Double-clicking the zip will not unzip it properly.
1. Open Easykext.app, and drag and drop every file in the unzipped `Release`
folder into it. Each time you drop, you will be prompted for your password.
There are 8 total kexts to be dropped into Easykext.app.
1. Restart OS X. Your menu bar should show a wifi logo, and you should be able to connec to any 2.4GHz SSIDs.
1. For 5GHz, unzip the [5-GHz-patch](#user-content-5-ghz-patch):

   ```unzip -d ~/ ~/Desktop/wireless_bcm94352-110-v4.0.command.zip```

   **Note**: This file must reside in your user home directory and not your desktop.
1. Then, run it:

   ```cd ~/; ./wireless_bcm94352-110-v4.0c.command```
1. Choose `Handoff/BCM94352/US-FCC` (I am located in the US).
1. Restart. You should see 5GHz SSIDs as well as "Allow Handoff between this Mac and your iCloud devices" checkbox preference in "System Preferences/General".

#### 4. Enable Bluetooth

1. Unzip the [Broadcom Bluetooth USB devices patch](#user-content-brcm-patch) zip file by the following:

   ```unzip -d ~/Desktop/BrcmPatchRAM ~/Desktop/RehabMan-BrcmPatchRAM-2015-1101.zip```

   **Note**: Double-clicking the zip will not unzip it properly.
1. Drop **BrcmPatchRAM2.kext** and **BrcmFirmwareRepo.kext** only to Easykext.app. Ignore other files.
1. Restart.

#### 5. Enable TRIM on SSD

1. Download [Disk Sensei](#user-content-disk-sensei).
1. Install and open it. Go to Tools/Trim. Turn it to "on."
1. Restart.
