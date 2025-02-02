
# **XeLL-Reloaded**

  

A Xenon Linux loader based on XeLL by tmbinc (Felix Domke)

  

XeLL-Reloaded catches CPU threads, sets them up, loads an ELF file from either network (tftp), USB(fat/ext2fs), CDROM (ISO9660) or HDD(fat/ext2fs/fatx), and launches it.

It's made to boot Linux, so it contains a flat device tree for Linux. However, it is able to load other ELF files as well, like applications based on LibXenon.

  

* based on LibXenon

* Supports Reset Glitch Hack - RGH. (xell-gggggg)

* XeLL-Reloaded is now divided in 2 stages:

	- 1st Stage initializes most of the Hardware, uncompresses and executes 2nd Stage

	- 2nd Stage (based on LibXenon) loads all required drivers and does the usual "XeLL tasks"

* XeLL can unzip & load gzipped files

* Support for HDMI, and properly switches NTSC/PAL on composite.

* All CPU Cores are active and ready to run at full speed.

* TinyEHCI is used, delivers full USB 2.0 speed when accessing mass storage media

* lwip network stack upgraded to v1.4 Final - It's faster and DHCP is improved.

* It can access the DVD-drive via DMA: faster reading

* It's possible to reload into XeLL when you are inside a LibXenon Application

* HTTP web interface to retrieve console's NAND dump.

* Improved hardware initialization now allows chain-loading.

* Supports upgrading with a 2-stage XeLL-Reloaded binary, named "updxell.bin"

* Infinite boot loop when looking for ELFs to execute.(no more rushing to get the live-cd in)

* Parses / decrypts keyvault

* Supports kboot.conf-type file

* Supports external initramfs

* Can pass a custom CMDLINE to Linux kernel via kboot.conf

kboot.conf/initrd support - copyright (C) 2010-2011 Hector Martin "marcan" <hector@marcansoft.com>

* Shows a user controllable menu for the parsed boot entries

XeLL user prompt - by Georg Lukas "Ge0rg" <georg@op-co.de>

  

# HOW TO USE

  
  

XeLL Reloaded checks for ELF/updxell.bin/kboot-config or updflash.bin in the following order:
USB (FAT/EXT2FS)

DVD(ISO9660)

HDD(XTAF/FAT/EXT2FS): 

	 updxell.bin

	kboot.conf

	xenon.elf

	xenon.z

	vmlinux

	updflash.bin



Network:

	updxell.bin

	kboot.conf

	xenon.elf

	xenon.z

	vmlinux

	updflash.bin (It will find that file, but refuse to flash it!)

	DHCP supplied bootfile-name


XeLL takes boot server from DHCP, if supplied. You can supply a static TFTP server IP via kboot.conf. If no TFTP server is found, it falls back to a static IP.

  

no updflash support via tftp !

updflash.bin is a already remapped flashimage/nandimage. 16MB file for 16MB NAND and 64MB file for 64/256/512MB NAND.

updxell.bin is a renamed xell.bin file. Version depending on the used hack

JTAG XeLL: (xell-1f oldies) (xell-2f modern),

RGH XeLL: (xell-gggggg)

  
  

kboot.conf (modified for XeLL) is a config file

  

General XeLL config: Set TFTP-Server, CPU-Speedup, videomode, NetConfig

  

Menu: Parses bootentries, shows a user controllable menu using an Xbox Controller, UART or IR Remote.

  

It can parse boot arguments for linux kernels & can load initramfs/initrd

  

xenon.elf/xenon.z/vmlinux can be either gzipped or bare ELF32 binaries - LINUX or Homebrew

  
  
  

There's also a HTTP Server running while XeLL searches for executable binaries. It can also serve the CPUKey/DVDKey and the console's flashdump.

  
  

# UPDATING XeLL

  
  

1. Rename the appropriate XeLL-binary to updxell.bin

2. Supply the updxell.bin file to XeLL via USB/DVD/HDD or TFTP

3. It should find the update and flash it

4. Reboot your Xbox and enjoy the fresh XeLL build.

  

Troubleshooting:

  

updxell.bin doesn't get found / updxell process doesn't start:

  

--You have to rebuild your whole hack image with a recent XeLL. From there on you can use the inbuilt update feature

updxell function reports that no XeLL binary was found in NAND:

  

--Either your XeLL in NAND is too old or it's not a XeLL Reloaded binary - You have to rebuild your whole hack image with a recent XeLL.

  
  

# FLASHING NAND

  
  

1. Rename the new (already remapped) flash image to "updflash.bin"

2. Supply the updflash.bin file to XeLL via USB/DVD/HDD

3. It should find the update and flash it

4. Reboot your Xbox and enjoy the new image

  

Troubleshooting:

  

Xbox does not boot properly after flashing the NAND:

--Either your image wasn't properly remapped or you made something wrong while building the image

  
  

# USING KBOOT.CONF

  
  

1. Read and understand the kboot.conf.sample which is part of every XeLL release with kboot-support

2. Modify the file to your needs

3. Supply it, named as "kboot.conf, via USB/DVD/HDD or TFTP

4. Boot XeLL and wait till it loads the file and shows the menu

5. Navigate to the desired boot entry. You can do this in the following ways:

UART:

UP/DOWN to go up and down in the menu

ENTER to confirm your choice

C to cancel the menu

  

X360 Controller (1st):

DPAD-UP/DPAD-DOWN to go up and down in the menu

A to confirm your choice

B to cancel the menu

  

X360 Media Remote:

UP/DOWN to go up and down in the menu

OK to confirm your choice

Press the NUMBER of the desired boot entry to directly load it

B to cancel the menu

  

6. Let the boot entry load. Enjoy :)

  


Troubleshooting:

kboot.conf gets found but it doesn't show boot entries or auto loads a boot entry:

--Make sure timeout is set to something higher than 0

--Make sure you didn't forget the "" on the bootentry: label="kernelpath params"

--Also take care of using a text editor which doesn't automatically break lines if they are too long (will break boot entries), also it shouldn't

modify the encoding and line endings of the config!
