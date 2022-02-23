# Hackintosh/Surfacebook 1 new Enhanced/OPENCORE 0.7.8
 Support Catalina-Bigsur-Monterey
 
 Hardware: CPU i5 6300U
 Ram: 8gb
 Graphics: Intel HD520
 Screen size: 3000x2000
 
 Install: Easy make USB Install Catalina-Bigsur-Monterey and mount EFI after this copy EFI folder in this. After install complete boot in Mac OS and used OpencoConfig mout EFI Internal HD and copy EFI folder to this and reboot

1) DRIVER GRAPHICS:

Search many tutorial for Surfacebook and Surface pro 4, i see all used Frambuffer 0x59160000 Kaby Lake system, my system is Skylake, what? in Catalina with 0x59160000 KB, graphic work norman, but in Bigsur and Monterey have problem view Youtube and Video file frezzee, system not Stanbility, i find this new Flatform ID Skylake work verygood on my surfacebook (and surfacepro 4)

The ig-platform-id is 0x59160000: 

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

DVMT pre of Surfacebook 32mb, I add maximum Stolen 30mb if >32mb will panic system, you can see:

<img width="662" alt="Screen Shot 2022-02-23 at 15 05 44" src="https://user-images.githubusercontent.com/100263269/155281223-b221a607-fd1c-4f0a-a4d9-b62ac9f1b59f.png">

Work Great in Skylake system no freezee, no glit in Hidpi, very stanbility

