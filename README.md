# Hackintosh/Surfacebook 1 new Enhanced/OPENCORE 0.7.8
 Support Catalina-Bigsur-Monterey
 
 Hardware: CPU i5 6300U
 Ram: 8gb
 Graphics: Intel HD520
 Screen size: 3000x2000
 
 Install: Easy make USB Install Catalina-Bigsur-Monterey and mount EFI after this copy EFI folder in this. After install complete boot in Mac OS and used OpencoConfig mout EFI Internal HD and copy EFI folder to this and reboot
 
 <img width="1800" alt="Screen Shot 2022-02-23 at 15 09 43" src="https://user-images.githubusercontent.com/100263269/155281660-aae24148-f79b-4c86-811f-a3dcc8237e2a.png">


1) DRIVER GRAPHICS:

All most laptop Skylake  and real Macbookpro13,1 used FB 19160000 but why Skylake Surfacebook and Surfacepro 4 used ig-platform-id is 0x59160000?

Search many tutorial for Surfacebook and Surface pro 4, i see all used Frambuffer 0x59160000 Kaby Lake system, my system is Skylake, what? in Catalina with 0x59160000 KB, graphic work norman, but in Bigsur and Monterey have problem view Youtube and Video file frezzee, system not Stanbility, i find this new Flatform ID Skylake work verygood on my surfacebook (and surfacepro 4)

The ig-platform-id is 0x59160000: found on intruction on Surface pro 4 and Surfacebook 1

ID: 59160000, STOLEN: 34 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00000B0B
TOTAL STOLEN: 35 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 103 MB, MAX OVERALL: 104 MB (109588480 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000800, flags: 0x00000187 - ConnectorHDMI
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00080000 87010000

See STOLEN: 34 MB, FBMEM: 0 bytes 

If you apply the "19MB Stolen + 9MB Cursor" or the "DVMT pre-alloc 32MB" patch, you can get rid of the kernel panic but probably will encounter a black screen with backlight only later.
Apparently, the above configuration does not work with Surface Pro, probably because the stolen memory is too small to light up your builtin high resolution display.

Remove any existing binary patch related to DVMT min stolen memory assertion.
Set framebuffer-stolenmem = 26MB, and do NOT change the cursor memory. i.e. Remove the field framebuffer-cursormem.
In your config.plist, the value of framebuffer-stolenmem should be 0x0000A001.

<img width="687" alt="Screen Shot 2022-02-23 at 15 00 26" src="https://user-images.githubusercontent.com/100263269/155280561-614e0c6e-be50-4622-bd87-18756e9e7d5b.png">

But with Skylake system we used Kaby Lake FB, it not good. I find  this FB of Skylake: 19160002

ID: 19160002, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00830B02
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel Iris Graphics 540
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98040000
01050900 00040000 C7030000
02040A00 00040000 C7030000

STOLEN: 57 MB, FBMEM: 0 bytes

DVMT pre of Surfacebook 32mb, I add maximum Stolen 30mb,if >32mb will panic system, you can see:

<img width="662" alt="Screen Shot 2022-02-23 at 15 05 44" src="https://user-images.githubusercontent.com/100263269/155281223-b221a607-fd1c-4f0a-a4d9-b62ac9f1b59f.png">

Work Great in Skylake system no freezee, no glit in Hidpi, very stanbility in Catalina, Bigsur and Monterey

<img width="597" alt="Screen Shot 2022-02-23 at 15 15 35" src="https://user-images.githubusercontent.com/100263269/155282388-f74943fd-0047-4f64-9a80-c3d77b52fde3.png">

2) DRIVER TRACKPAD:

![Screen Shot 2022-02-23 at 15 28 14](https://user-images.githubusercontent.com/100263269/155284049-fd8b03d8-11a0-4073-87c9-001a8d240f27.png)

Catalina and Bigsur you can used patch VoodooI2C for multitouch trackpad from Bigsadan

<img width="944" alt="Screen Shot 2022-02-23 at 15 29 26" src="https://user-images.githubusercontent.com/100263269/155284305-81d6da4a-8036-4a98-989c-d73a26e56e22.png">

But it not work in Monterey, i found new kext modify from Surface pro 7 : BigSurface.kext (many thank)

<img width="947" alt="Screen Shot 2022-02-23 at 15 32 32" src="https://user-images.githubusercontent.com/100263269/155284590-e2c71424-03f6-49d0-93a5-f3d6a3f7e5d8.png">

3) DRIVER BATTERY

I see from DSDT bigsadan for surfacebook with dual battery, but not good, many error in bootlog and not correct info, i Used ECenable.kext and ACPIbattery from Rebhaman for battery, it work verygood but not detect Charger when plug and unplug, i see when install ECenable, it modify my DSDT and some code will change from devide ADP1, i modifed it to original and AC Adapter work great for remain charg and remain when used battery.

4) POWER MANAGERMENT

With SSDT-DATA + CPUfriend degree CPU default from 1,3 to 900mz, system work for balance enegy and with correct FB Skylake Intel HD 520, work same Macbookpro13,1

AUto Dim when unplug charg and Low Power mode enable

<img width="722" alt="Screen Shot 2022-02-23 at 15 43 23" src="https://user-images.githubusercontent.com/100263269/155286060-033ac748-11d2-4b73-befd-985403ad2b91.png">








