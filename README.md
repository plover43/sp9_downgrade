## SP9 UEFI Firmware Downgrade Tutorial

#### Disclaimers
This is probably risky and should not be attempted. After using the result for several hours I have yet to experience any issues, however this could change. 

#### Background:
https://github.com/linux-surface/linux-surface/issues/1162

These are the steps I took to downgrade my surface pro 9 with uefi fw 12.200.143 to 9.12.143 to enable the installation of fedora 40 because ubuntu sucks. 
The original cab from MS contains two firmwares. During my testing I tried installing both but only the first one installs. It appears as though the second firmware is for different hardware, perhaps the arm version of the SP9. 
Special thanks to Dorian Stoll (@StollD) and @Ramen-LadyHKG their pointers in the matrix chat


### Steps to produce a working sp9 cab downgrade file

#### Prereqs 
1. Install ubuntu or any distro that works on the sp9
2. Install dependencies and clone https://github.com/linux-surface/surface-uefi-firmware
3. Obtain the last working firmware from the MS archive site
https://www.catalog.update.microsoft.com/Search.aspx?q=9.12.143.0

#### cab creation
1. Extract all contents of the downloaded cab file. I already had the files from running `./repack.sh -f e4c25701-a413-40b6-bca4-9651ea1906c9_9f10207676fea752fa15d8ec28fdd22042b560d6.cab` so I simply used the result of the failed run

The result of the extraction will be as follows:

├── SurfaceUEFI_9.12.143_0.bin

├── SurfaceUEFI_9.12.143_0_integrity.bin

├── SurfaceUEFI_9.12.143_1.bin

├── SurfaceUEFI_9.12.143_1_integrity.bin

├── surfaceuefi.cat

└── SurfaceUEFI.inf

2. Copy SurfaceUEFI_9.12.143_0.bin, surfaceuefi.cat into a new working directory

3. Rename SurfaceUEFI_9.12.143_0.bin to firmware.bin and surfaceuefi.cat to firmware.cat

4. Copy firmware.metainfo.xml and firmware.inf files from this repo into the working directory

5. Generate the cab file: `gcab -c SurfacePro9UEFI_9.12.143_0.cab ./*`
6. Install cab file `sudo fwupdmgr install --allow-older --allow-reinstall --force SurfacePro9UEFI_9.12.143_0.cab`
7. Reboot to install the new firmware

