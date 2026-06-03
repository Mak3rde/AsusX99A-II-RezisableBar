# Resizable BAR / Smart Access Memory on ASUS X99-A II

Notes, files, and steps for enabling Resizable BAR / Smart Access Memory on the
ASUS X99-A II by using a modified BIOS with ReBarUEFI.

![Resizable BAR screenshot](https://user-images.githubusercontent.com/16582202/201349183-7a76d8a5-3e3a-4b59-bf30-429b5067ad57.png)

## Warning

BIOS modding can brick your motherboard.

- This guide is for the ASUS X99-A II.
- The ready-made files linked below are based on BIOS version `2101`.
- Make a full BIOS backup before changing anything.
- Use this at your own risk.
- BIOS Flashback is the recommended flashing method.
- I do not recommend using AFUWin for flashing. Use AFUWin only for making a backup, if at all.
- If you cannot recover with BIOS Flashback, you may need an external SPI programmer and a second computer.

Read the full guide once before starting.

## Compatibility

| Item | Status |
| --- | --- |
| Mainboard | ASUS X99-A II |
| BIOS version | 2101 |
| Feature | Resizable BAR / Smart Access Memory |
| Flash method | BIOS Flashback recommended |

If you tested this with a specific CPU/GPU combination, document your result in
an issue or pull request.

## Credits And Sources

This would not have been possible without information gathered from multiple
sources and help from `icymiguel420` on the Miyconst Hardware Discord.

- Resizable BAR on LGA 2011-3 X99 - how to enable and get extra performance:
  <https://www.youtube.com/watch?v=vcJDWMpxpjE>
- Miyconst Hardware Discord:
  <https://discord.gg/F5qMzdsQGX>
- xCuri0 / ReBarUEFI:
  <https://github.com/xCuri0/ReBarUEFI>
- LongSoft / UEFITool:
  <https://github.com/LongSoft/UEFITool/releases/tag/0.28.0>
- Win-Raid Forum thread:
  <https://winraid.level1techs.com/t/asus-x99-aii-unlock-msr-0xe2-and-native-nvram/36830>
- ASUS X99-A II BIOS downloads:
  <https://www.asus.com/us/supportonly/x99-a%20ii/helpdesk_bios/>

## Ready-Made Downloads

These files were attached to the original GitHub repository.

- [X99-A-II-ASUS-2101_CAP_STOCKROM_64-bit_BAR_ENABLED_rebar_driver_injected_barsize_Patched.zip](https://github.com/Mak3rde/AsusX99A-II-RezisableBar/files/9990127/X99-A-II-ASUS-2101_CAP_STOCKROM_64-bit_BAR_ENABLED_rebar_driver_injected_barsize_Patched.zip)
- [X99-A-II-ASUS-2101_ROM_STOCKROM_64-bit_BAR_ENABLED_rebar_driver_injected_barsize_Patched.zip](https://github.com/Mak3rde/AsusX99A-II-RezisableBar/files/9990130/X99-A-II-ASUS-2101_ROM_STOCKROM_64-bit_BAR_ENABLED_rebar_driver_injected_barsize_Patched.zip)

Recommended improvement: move these files to GitHub Releases and add SHA256
checksums.

## Required Tools

- ASUS X99-A II BIOS file
- MMTool
- UEFITool and UEFIPatch
- `patches.txt`
- `ReBarDxe.ffs`
- patched `PciBus.ffs`

Downloads used by the original guide:

- [patches_txt.zip](https://github.com/Mak3rde/AsusX99A-II-RezisableBar/files/9990133/patches_txt.zip)
- [X99-A-II-ASUS-2101_pciebusffs_rebardxeffs.zip](https://github.com/Mak3rde/AsusX99A-II-RezisableBar/files/9990135/X99-A-II-ASUS-2101_pciebusffs_rebardxeffs.zip)

Put all files in the same working directory.

## DIY Patching Guide

### 1. Prepare

1. Download the latest ASUS X99-A II BIOS from ASUS.
2. Download MMTool, UEFITool, and UEFIPatch.
3. Download `patches.txt`.
4. Download `ReBarDxe.ffs` and `PciBus.ffs`.
5. Make a BIOS backup before continuing.

### 2. Replace PciBus With MMTool

1. Open MMTool.
2. Load the ASUS BIOS `.CAP` file.
3. Open the `Replace` tab.
4. Find `PciBus` in `Volume 02`.
5. Select `PciBus`.
6. Choose the patched `PciBus.ffs` as the module file.
7. Click `Replace`.

![MMTool replace screenshot](https://user-images.githubusercontent.com/16582202/201347829-756f5562-81a5-4c73-9114-1b8350e35977.png)

Alternative: instead of replacing `PciBus.ffs`, add this patch to `patches.txt`.
The last character in the line must be a space.

```text
#PciBus | Don't downgrade 64-bit BARs to 32-bit
3C1DE39F-D207-408A-AACC-731CFB7F1DD7 10 P:C70605000000833E067506C70604000000BE01000000:909090909090833E067506909090909090BE01000000 
```

### 3. Insert ReBarDxe

1. In MMTool, switch to the `Insert` tab.
2. Choose `ReBarDxe.ffs` as the module file.
3. Enter `02` as the volume index.
4. Click `Insert`.
5. Save the image.
6. Close MMTool.

![MMTool insert screenshot](https://user-images.githubusercontent.com/16582202/201348146-2c96e1a9-1eb8-4b4c-a09a-ebe1e9a04534.png)

### 4. Apply The NVRAM Whitelist Patch

Use the attached `patches.txt`, or create a new text file with this content:

```text
#NVRAM whitelist unlock
54B070F3-9EB8-47CC-ADAF-39029C853CBB 10 P:0F84B300000041F6:90E9B300000041F6 
```

The line must end with a space.

Run UEFIPatch from a command prompt in your working directory:

```cmd
uefipatch BIOSFILENAME.CAP -o PATCHED_BIOS_FILENAME.CAP
```

### 5. Flash The BIOS

Preferred method:

1. Flash the patched `.CAP` file with the motherboard BIOS Flashback feature.

Alternative method:

1. Open the patched `.CAP` file in UEFITool.
2. Use `Action -> Capsule -> Extract body`.
3. Save it as `.bin` or `.rom`.
4. Flash the `.bin` or `.rom` directly to the BIOS chip with an external programmer.

## CAP vs ROM

- Use `.CAP` for ASUS BIOS Flashback.
- Use `.ROM` or `.BIN` for direct chip flashing with an external programmer.
- Do not randomly rename files unless the BIOS Flashback instructions require a specific filename.

## Troubleshooting Flashback

If BIOS Flashback does not start or appears to fail:

1. Make sure the USB stick is formatted as FAT32.
2. Use the dedicated BIOS Flashback USB port.
3. Use the filename required by ASUS BIOS Flashback for the X99-A II.
4. Confirm that the file is a valid `.CAP` file, not a raw `.ROM`.
5. Try a smaller or older USB 2.0 stick.
6. Wait until the Flashback LED stops blinking.
7. Clear CMOS after flashing.
8. If the board no longer boots, recover with BIOS Flashback or an external SPI programmer.

## After Flashing

Check your BIOS and operating system settings:

- Enable Above 4G Decoding if available.
- Enable Resizable BAR / Smart Access Memory support where available.
- Check CSM/Secure Boot settings if the system does not boot as expected.
- Verify in the GPU driver or tools such as GPU-Z that Resizable BAR is active.

## Repository Maintenance Notes

Recommended future cleanup:

- Move downloadable BIOS ZIPs into GitHub Releases.
- Add SHA256 checksums for every BIOS-related ZIP.
- Add tested CPU/GPU combinations.
- Add exact BIOS Flashback filename requirements for the X99-A II.
- Keep third-party tools linked instead of vendored.

## License

The written documentation in this repository is licensed under CC BY 4.0. Third-party tools, BIOS files, firmware modules, screenshots, and linked downloads remain under their respective owners' licenses and terms.
