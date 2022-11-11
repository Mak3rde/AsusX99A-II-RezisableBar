# AsusX99A-II Working RezisableBarwith / Smart Acces memory with Unlimited Size.

## This wouldn't be possible without the information gathered from multiple sources and the help of icymiguel420 (Miyconst Hardware Discord)

# Sources:

Resizable BAR on LGA 2011-3 X99 – how to enable and get extra performance
- https://www.youtube.com/watch?v=vcJDWMpxpjE ( Discord: https://discord.gg/F5qMzdsQGX ) 

xCuri0 / ReBarUEFI

- https://github.com/xCuri0/ReBarUEFI

LongSoft / UEFITool
- https://github.com/LongSoft/UEFITool/releases/tag/0.28.0

Win-Raid Forum
- https://winraid.level1techs.com/t/asus-x99-aii-unlock-msr-0xe2-and-native-nvram/36830



# Notice:

## MAKE THE BIOS BACKUP FIRST

- I would not recommend using afuwin to flash the uefi, my recommendation is clearly to use afuwin only for uefi backup.
- Flashing with the BIOS-FLASHBACK from the mainboard is easy and safe.
- Alternatively, you can flash the bioschip directly using a flasher, but this requires extra hardware and a second computer.

Tools: 
- MMtool
- uefitool + uefipatch -> https://github.com/LongSoft/UEFITool/releases/tag/0.28.0
- bios file -> https://www.asus.com/us/supportonly/x99-a%20ii/helpdesk_bios/

### ready to use efi Downloads:  
- [X99-A-II-ASUS-2101_CAP_STOCKROM_64-bit_BAR_ENABLED_rebar_driver_injected_barsize_Patched.zip](https://github.com/Mak3rde/AsusX99A-II-RezisableBar/files/9990127/X99-A-II-ASUS-2101_CAP_STOCKROM_64-bit_BAR_ENABLED_rebar_driver_injected_barsize_Patched.zip)
- [X99-A-II-ASUS-2101_ROM_STOCKROM_64-bit_BAR_ENABLED_rebar_driver_injected_barsize_Patched.zip](https://github.com/Mak3rde/AsusX99A-II-RezisableBar/files/9990130/X99-A-II-ASUS-2101_ROM_STOCKROM_64-bit_BAR_ENABLED_rebar_driver_injected_barsize_Patched.zip)


# If You want to diy 

### How To:

## 1 
- Download Latest Bios From Asus website 
- download MMtool, Uefi tool and uefipatch
- download patches.txt
[patches_txt.zip](https://github.com/Mak3rde/AsusX99A-II-RezisableBar/files/9990133/patches_txt.zip)

- download rebardxe.ffs and pcibus.ffs 
- [X99-A-II-ASUS-2101_pciebusffs_rebardxeffs.zip](https://github.com/Mak3rde/AsusX99A-II-RezisableBar/files/9990135/X99-A-II-ASUS-2101_pciebusffs_rebardxeffs.zip)
- put all files in same directory 

### MAKE THE BIOS BACKUP FIRST

## 2
- Open Mmtool, 
  - load Image -> ASUS_BIOS.CAP
  -  Replace Tab 
  -  scroll down until you find "Priebus" in Volume 02
  - click on pciebus
  -  look for "Module file" further up the window
  - now choose the pcibus.ffs by clicking on BROWSE
  -  click replace
 ### 2b
  -  switch to Insert Tab 
  -  look for "Module file" further up the window
  - now choose the rebar.ffs by clicking on BROWSE
  - directly below, enter "02" at Vol. index
  -  click insert
  
 ### 2c
 - click "save Image"
 -  close mmtool

 ## 3 
- download the atached patches.txt 

### OR

-create a new txt file  in you folder 
-  paste
#NVRAM whitelist unlock
54B070F3-9EB8-47CC-ADAF-39029C853CBB 10 P:0F84B300000041F6:90E9B300000041F6_
- replace the _ at the ende ( 1f6_) with a "Space "
- save file and close it.

## 4
- press Win+R on your Keyboard 
-  or start and search for cmd
- change directory to your folder  
- then type:  uefipatch BIOSFILENAME.CAP -o PATCHED_BIOS_FILENAME.CAP
- press enter

## 5
 ### A: 
- flash that cap file using the bios flashback feature 
### OR
### B: 
- open Efitool, load Patched .cap File 
- Click -> Action -> Capsule -> Extract body -> save as .bin or .rom
- flash to bios 



**System**
 - Motherboard: Asus X99A-2 (X99-A II)
 - BIOS Version: 2101 (type: UEFI)
 - GPU: PowerColor AMD Radeon RX 6950 XT
 - Operating System: Windows 10 Pro 64-bit (10.0, Build 19045) (19041.vb_release.191206-1406)
