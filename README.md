# OpenCore EFI for DELL OptiPlex 3060M
This build is based on OpenCore v0.6.2

## Index
* [Hardware](#hardware)
* [Tools](#tools)
* [BIOS Settings](#bios-settings)
* [Find DVMT pre-alloc and CFG Lock NVRAM store address](#find-dvmt-cfg-lock-address)
* [Known Issues](#known-issues)

## Hardware
To make WiFi and handoff work natively I have installed Apple genuine BCM94360CS2 with a m.2 NGFF adapter.

Avoid Samsung PM981 series SSD.

## Tools
- flashrom - Create backup of the current BIOS
- UEFITool - Search and extract ```setup``` region of the BIOS
- IFRExtractor - Convert the ```setup``` region to human readable text

## Find DVMT pre-alloc and CFG Lock NVRAM store address
1. Boot any Linux distro and install flashrom and backup the BIOS.
2. Open the BIOS image by UEFITool.
3. Use the search function to search for "DVMT" or "CFG Lock" as text.
4. Double click the found address at the bottom of UEFITool window.
5. Open the node and right click on the "PE Image" and select "Extract as is"
6. Open the file by IFRExtractor and convert the extracted file from step 5 to text format
7. Open the text file and search for DVMT pre-alloc and CFG Lock
8. Write down the value store addresses and the value to set. In my case DVMT pre-alloc is at ```0x8DC``` and the 64MB option value is ```0x2```. CFG Lock is at ```0x5BE``` and disabled flag is ```0x0```.

## BIOS Settings
Load default settings and disable VT-d.

## CFG Lock
Boot the OpenCore EFI and select ```modGRUBShell```. Enter the following command when the ```grub>``` prompt appears.

```
grub> setup_var 0x5BE 0x0
```

## DVMT pre-alloc
If you wish to connect your hackintosh to a 4K display, you must set the DVMT pre-alloc value to 64MB+. Otherwise you are fine with the ```framebuffer-stolenmem``` patch.

Boot the OpenCore EFI and select ```modGRUBShell```. Enter the following command when the ```grub>``` prompt appears.

```
grub> setup_var 0x8DC 0x2
```

## Known Issues
Front panel microphone jack has not been tested.