# Hackintosh Guide for **ASUS VivoBook 15 Pro N580GD** 

**EFI is made with OpenCore 0.7.0 and tested with macOS Big Sur 11.4**

## Info:
- I replaced the motherboard in my N580VD laptop with a N580GD board, so your specs might differ.
- I replaced the stock WIFI module with a DW1820A.  
If you have a different WIFI card delete: 'AirportBrcmFixup and BrcmPatchRAM kexts' and do your own research.
- Users with a I7-8750H CPU shoud re-build CPUFriendDataProvider.kext as it has a different TDP-down value than I5-8300H.
- You need to edit SMBIOS settings for iServices to work. Refer to [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/extras/smbios-support.html)
- I added VoltageShift.kext to undervolt the CPU. Remove it if you don't want to undervolt. More details below.

## Specs:
| Component | Name |
|:--- |:---:|
| Motherboard:  | N580GD |
| CPU: | Intel i5-8300H |
| Chipset: | HM370 |
| RAM: | 32GB (2 x Hynix HMA82GS6JJR8N-VK 16GB DDR4 SODIMM 2666MHZ) |
| iGPU: | Intel UHD 630 (Mobile) |
| dGPU: | NVIDIA GeForce GTX 1050 4GB (DISABLED) |
| SSD: | 256GB Micron 1100 M.2 (MTFDDAV256TBM) |
| HDD: | 2TB FireCuda (ST2000LX001) |
| Wifi/BT: | Dell DW1820A CN-0VW3T3 BCM94350ZAE |
| Audio: | Conexant Audio CX8150 |
| Ethernet: | Realtek RTL8168H |
| Trackpad: | ELAN1200 Precision TouchPad |
| Keyboard: | Standard PS/2 Keyboard |
| Built-in Display: | B156HAN06.1 (1080p non touch) |

### Working:
- [x] **Tested with Big Sur 11.4**
- [x] **Wifi & Bluetooth** Thanks to AirportBrcmFixup and BrcmPatchRAM.  
(Removed AirPortBrcm4360_injector.kext from AirportBrcmFixup.kext)
- [x] **Audio:** Thanks to AppleALC.kext with layout-id=21 in Device Properties.
- [x] **USB & Webcam:** All internal and external ports (Thanks to SSDT-EC-USBX-LAPTOP.aml & USBmap.kext)
- [x] **Ethernet:** Realtek RTL8168H (Thanks to RealtekRTL8111.kext)
- [x] **Sleep/Wake:** Thanks to CPU Powermangement, USB mapping and NVMe Power management (NVMeFix.kext)
- [x] **CPU Power managemnt:** Thanks to CPUFriend.kext & CPUFriendDataProvider.kext
- [x] **NVRAM:** Thanks to SSDT-PMC.aml.  
Shouldn't be required on HM370 chipsets (according to Dortania), but NVRAM doesn't work without.
- [x] **System Clocks:** Thanks to SSDT-AWAC.aml.  
Shouldn't be required on HM370 chipsets (according to Dortania), but doens't boot without.
- [x] **Function Keys:** Thanks to MaLd0n from Olarila's DSDT patches.
- [X] **Trackpad:** Thanks to MaLd0n from Olarila's DSDT patches.
- [X] **HDMI:** HDMI video out works thanks to framebuffer-con2-alldata patch under 'PciRoot(0x0)/Pci(0x2,0x0)'.

### Not working: 
- [X] **dGPU:** Since 2015, Apple has stopped using Nvidia graphics chips in Macs.
- [ ] **HDMI:** Audio over HDMI, needs more testing.

### Untested:
- [ ] **HDMI:** Video-out over USB-C. Enable framebuffer-con1-alldata patch under 'PciRoot(0x0)/Pci(0x2,0x0)' to test.
- [ ] **Cardreader:** I don't use it, so I didn't test it yet.
- [ ] **Keyboard Backlight FN keys:** I didn't attach the KBL to my motherboard so I can't test it.

### Under-volting:

WARNING! UNDERVOLTING SHOULDN'T BE DANGEROUS, BUT IF YOU DON'T KNOW WHAT YOU'RE DOING YOU MAY DAMAGE YOUR LAPTOP. USE AT YOUR OWN RISK!!! 
DO NOT TRY THE OVERCLOCKING UNLOCK OFFSET ON A DIFFERENT BIOS VERSION OR LAPTOP!!! 

I undervolted my machine to decrease the CPU temperatures. In order to undervolt you need to disable overclocking-lock in BIOS. Since this option is hidden in the GUI on our systems, you need to extract the offsets with UEFITool and IFR-Extractor. On FW324 you can disable the Overclocking-lock by booting ModifiedGRUBShell.EFI and typing 'setup_var 0xB4B 0x00'  

I've tested this undervolt in Windows with ThrottleStop first, torture tested for 16 hours in Prime95 on small FFT's, did some video encoding with Handbrake and used the system for about 2 weeks without any BSOD. I then tested these values in MacOS for a couple of days to test system stability. I recommend you try starting with a -100mV CPU and CPU Cache offset. 

Download the [Voltageshift script](https://github.com/sicreative/VoltageShift) place it under '/Library/Scripts/VoltageShift' and test thoroughly before applying these values at boot.

I set these values at boot with: 'sudo ./voltageshift buildlaunchd -165 -70 -165 -70 0 0 1 -1 -1 -1 0'

- CPU voltage offset: -165mv.  
- GPU voltage offset: -70mv.  
- CPU Cache voltage offset: -165mv.  
- System Agency offset: -70mv.  
- Analogy I/O: 0mv.  
- Digital I/O: 0mv.
- Turbo: On
