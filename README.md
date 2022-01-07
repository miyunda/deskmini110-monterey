Build a hackintosh with AsRock Deskmini 110
# Environment
MB: AsRock Deskmini 110/COM

CPU: Kaby Lake desktop (i?-7?00)

GPU: HD630

Wi-Fi and BT: DW1560

Ethernet: Intel I219V2 (shipped with MB)

NVME: Samsung SM951 256GB

# Results:
## Tested and work:
- [x] CPU SpeedStep
- [x] GPU acceleration
- [x] Wi-Fi
- [x] BT
- [x] 1Gbps Ethernet
- [x] USB ports
- [x] DP output to a 4K display
- [x] Continuity (AirDrop, Handoff, etc)
- [x] iMessage & Facetime
- [x] HEVC hardware decode
## Not tested:
- [ ] HDMI
- [ ] 3.5 mm jack
- [ ] DRM playback (It is a headless)
- [ ] VGA output(Guess it won't work)
## Does not work:
- [ ] Sleep (Didn't test, I am sure it would fail on wakeup)


# Install
Get a USB disk (capacity >= 16GB). Be patient, you Mac (the one you are using to make installer) might be picky, treat it, buy a new USB disk.

Follow [OpenCore Installation Guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/#creating-the-usb).

TL;DR: All you need are:

- Download the Monterey full Installer from Apple.
- Write it to a USB disk.
- Mount the USB disk's EFI volume by means of [MountEFI](https://github.com/corpnewt/MountEFI).
- Generate [SMBIOS](https://github.com/corpnewt/GenSMBIOS)
- Download this repo, rename `EFI-deskmini110-KBL-dw1560-Monterey` to `EFI`
- Replace `SystemProductName`, `SystemSerialNumber`, `MLB` and `SystemUUID` in `EFI/OC/config.plist` with your just generated values.
- Copy **entire** `EFI` folder to the USB disk's EFI volume.
- Adjust BIOS settings. 
- Install macOS.
- Post-install: mount both EFI volumes of the USB disk and internal HDD, copy `EFI` folder from the USB disk to internal HDD.
---
About BIOS settings:

Be sure to Disable:
- [ ] Fast boot
- [ ] Secure boot
- [ ] CSM
- [ ] Intel SGX
- [ ] Intel Platform Trust
- [ ] Serial port

Enable:
- [x] VT-D
- [x] Hyper-Threading
- [x] XHCI Hand-off
- [x] DVMT Pre-Allocated(iGPU Memory): 64MB

You are done, enjoy.
Thanks to
[@dortania](https://github.com/dortania)
[@corpnewt](https://github.com/corpnewt)
[@acidanthera](https://github.com/acidanthera)
[@xRetia](https://github.com/dfc643)
