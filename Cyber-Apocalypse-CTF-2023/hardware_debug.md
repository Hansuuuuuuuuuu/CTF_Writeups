# Hardware 3: Debug
> "Your team has recovered a satellite dish that was used for transmitting the location of the relic, but it seems to be malfunctioning. There seems to be some interference affecting its connection to the satellite system, but there are no indications of what it could be. Perhaps the debugging interface could provide some insight, but they are unable to decode the serial signal captured during the device's booting sequence. Can you help to decode the signal and find the source of the interference?"


#### Difficulty: easy

We are given a zip file that contains a file: `hw_debug.sal`. I opened the file using `Logic 2` and got the ballpark baud rate using the `Baud rate estimate` extension, which gave me `115.741 kHz`. I then cross-referenced [Lucidar.me](https://lucidar.me/en/serialib/most-used-baud-rates-table/) for the common baud rates, where I found the closest one to be `115200`.

I then applied the Async Serial analyzer with the aforementioned baud rate. This gave me legible output on the data table, but upon switching to the terminal view, things became clearer:

<details><summary>Terminal output</summary>

```
\xC3\x1B\xEC\xDF\x86z^B#\xFF_@L\xA5C`B\xE8\xC3\xC5\xC3pO\xD7\xFECpA\xC2\xCEH\xFF\xC3\xCE\x84\xDB\xC3\x85\xE2\xBD\xE8\xC3\xCEH\xFF\xC3\xCE\x84\xDB\xC3\x84\xE3\xBD\xE8\xC3\xCEH\xFF\xC3\xCE\x84\xDA\xC3\x84\xE3\xBD\xE8\xC3\xCFH\xFF\xC3\xCE\x84\xDB\xC3\x84\xE3\xBD\xE8\xC3\xCEH\xFF\xC2\xCE\x84\xDB\xC3L\xF3C\x86\xFEB`3\xFEC`\xFF\xC2\xDE\xB1\xA7\xFC\xC3\xE4\xCF\xA5R\xFB\xC3\xDD\xD8\xADC\xFD\xC3\xBE\xFFC\xBE\xFF\xC3\xDE\xB3\xA7\xFC\xC3\xCC\xD8\xADB\xFD_`\xFFC\x1D\x97\xF3^s9\xFF^C/\xA4\xDF\xE4\xC7_B\x1B_@k\xA5_\xA7b)\xDF\xC5\x7F\xF6\xEB\xAE\xB6\xFEC\xE5\xEB\xC6\xE4^\x82F\xE6k\xFF_C\xAB\xA4\xFF\xFFN\xE7BB\x0C\xFD\xFFB\xF9c"B\xFFo\xFF\xE7\xE4\xFF\xFFc\xFC\xE3\xFE\xE2\xFE^Bb\xF0\xEF\x85]\xF7\xFF\x1A\xDE\xDBS\xFF\x1A\xDE\xDBSS\xFAc\xFFb\xA4wc\xF3\xFF\xFFBb\xFFB2Br\xFF\x8B\xFF\xFBZa\xFF\xF9Br!\xFABB\xFFBrrb2Zc\xFFB2bBrBr\xFFB2\xFF\xEB\x86\xFB\xDEc\xEEc\xEFc\xEF\xCA\xE5\x08\xD6\xDC\xFE\xDEc\xEFc\xEFc\xEE\xFF\xFF\x1A\xDE\xDBS\xFF\x1A\xDF\xDASg\xBF\xFBc\xA4\xC7\xFFkP\xA4
C\x87\xFFf\xE6\xEE\x15\xFEf\x1F\xA6\xC7\xFFf\\x96\xBD\xB2\xE2\xA6\xF3\xFE\xEA\xC6\xCA\xFE\xF2\x90N\xB6zb\xF7\xE2\xC6S\xEA\x16\xE2\xEA\xA6\xF2\xF3r\xBE\xFDZb\xFF\xEA\x9E\xE2\xEA\xC6\xCBjL\xE2\xE2\xE7\xFEb\xE7BY\xFE\xA4o\xFEf\\x96\xF0f\xCE@\x15\xFEf\x1F\xA6\xC7\xFF\xE2\xAE#\xFE\xEA\xC6
\xFC\xF2\x90N\xB4z\x02\xB1\xE2\xC6S\xEA\x16\xEA\xEA\x86\xF2\xF3r\xBE\xFDZb\xFF\xEA\x1F\xE2\xEA\xC7\xCAj\xCC\xE2\xE2\xE7\xFEb\xE72S\xF1C\x08\xFCf\\x96\xF0f\xCEs\x9C\xFEfX\xCE\xC8f\x1F\xA6\xC7\xFF\xE2\xF5\xDB\xE4\xFB\xEA\xE6Rt\xFF\xF2\x90N\xB4zBB\xFF\xE2\xE6\xFA\xEA\x16\xE6\xEA\xA6\xDA\xF3r\xBE\xFD\xE2\xE7\xFEb\x87HC9\xC7\xB0\xFEf\\x96\xF0f\x98\xCFC~2\xC6\xA4\xE2\xFEs\xFFf-R\x1A\xF6fXSb\x94\x8C\xFFN\xE5k\xCF\xC8\xFE\xFF\xDEc\xEF\xFB\xF2U\xCE\xFA\xDEc\xEF\xFB\xFFN\x1A\xD7\xF3\xF6\xE2\x84s\xD3\x9E\xB5O\xCF\x7F\x9Fz\xEEO}\xE2yo\x86\x11\xFA\xFFB\xF1B\x10\xFEBB\xB3BBIBBsbB\x10\xFFBb\x02\xFEBBB\xFDB\x10b\xFABC\x08\xF2B\x86B\xFABrB\xFABS\x08\xFABBR\xFDB\xE4F\xFFB\x08\xCABA\xF3\xE2\xC7\xE6\xFFNb\x1D\xFEBbA\xF9C\x02p\xFFBR\xF2BB\xF9bb\xFFB\xF0B\xFC\xFF\xE3\xD6\xFF{b\xE4\xEF\x84\xFB\xEBg\xFF\xC7\xD7\x88S\xF9c\xFFINFO:    AUTOSTARTUP MODE = 1
NOTICE:  Booting Trusted Firmware
NOTICE:  BL1: v1.3(release):f26889a
NOTICE:  BL1: Built : 13:04:48, Jul 30 2018
INFO:    BL1: RAM 0x30200000 - 0x30209000
INFO:    Using crypto library 'STM ATF Mini Certificates Crypto Library'
INFO:    BL1: Searching FIP image
INFO:    search_fip start,  slot: 0
INFO:    search_fip end,  slot: 0, result: 0
INFO:    VALID code at slot 0 ref_ctr 0x5555 (21845)
INFO:    search_fip start,  slot: 1
INFO:    search_fip end,  slot: 1, result: 0
INFO:    VALID code at slot 1 ref_ctr 0x5555 (21845)
INFO:    search_fip start,  slot: 2
INFO:    search_fip end,  slot: 2, result: 0
INFO:    VALID code at slot 2 ref_ctr 0x5555 (21845)
INFO:    search_fip start,  slot: 3
INFO:    search_fip end,  slot: 3, result: 0
INFO:    VALID code at slot 3 ref_ctr 0x5555 (21845)
INFO:    search_fip start,  slot: 4
INFO:    search_fip end,  slot: 4, result: 0
INFO:    TERM code at slot 4
INFO:    Chosen slot = 3
INFO:    BL1: Get the image descriptor
INFO:    BL1: Loading BL2
INFO:    Loading image id=6 at address 0x30209000
INFO:    Skip reserving region [base = 0x30209000, size = 0x90]
INFO:    Image id=6 loaded at address 0x30209000, size = 0x90
INFO:    cert_nv_ctr : 1
INFO:    plat_nv_ctr : 0
INFO:    Loading image id=1 at address 0x30209000
INFO:    Image id=1 loaded at address 0x30209000, size = 0xf178
NOTICE:  BL1: Booting BL2
INFO:    Entry point address = 0x30209000
NOTICE:  BL20: v1.3(debug):25afa823
NOTICE:  BL20: Built : 16:55:25, Jul 17 2020
NOTICE:  EMMC boot counter is 47
NOTICE:  BL2: v1.3(debug):4ac84b3
NOTICE:  BL2: Built : 15:39:21, Oct 15 2020
NOTICE:  Evaluate 0x8110010 & 0x10 == 0x0 -> 0
NOTICE:  Using default targetpack config
Boot firmware v20190415-1 starting
!*:#)$<@[|{!!("!%5!#!!"O-#A!!!"
Boot firmware exiting normally
NOTICE:  BL2: end TP
NNOTICE:  BL31: v1.3(debug):4ac84b3
NOTICE:  BL31: Built : 15:39:24, Oct 15 2020


U-Boot 2016.05-00307-g16c388c (Jul 23 2021 - 22:19:05 +0000)

DRAM:  1004 MiB
MMC:   Fast boot:eMMC: 8xbit - div2
stm-sdhci0: 0
Using default environment

In:    nulldev
Out:   serial
Err:   serial
CPU ID: 0x00020101 0x09011010 0xb9258380
Detected Board rev: #rev1_pre_production
sdhci_transfer_data: Error detected in status(0x208000)!
sdhci_transfer_data: Error detected in status(0x208000)!
FIP1: 3 FIP2: 3
BOOT SLOT B
Net:   Net Initialization Skipped
No ethernet found.


                }--O--{
                  [^]
                 /ooo\
 ______________:/o   o\:______________
|=|=|=|=|=|=|:A|":|||:"|A:|=|=|=|=|=|=|
^""""""""""""""!::{o}::!""""""""""""""^
                \     /
                 \.../
      ____       "---"       ____
     |\/\/|=======|*|=======|\/\/|
     :----"       /-\       "----:
                 /ooo\
                #|ooo|#
                 \___/

Is production hardware? YES
=============================================================================
= Type 'egypt' to stop boot process in the next 2 seconds =
=============================================================================

sdhci_transfer_data: Error detected in status(0x208000)!
sdhci_transfer_data: Error detected in status(0x208000)!
switch to partitions #0, OK
mmc0(part 0) is current device

MMC read: dev # 0, block # 98304, count 49152 ... 49152 blocks read: OK
## Loading kernel from FIT Image at a2000000 ...
   Using 'rev1_pre_production@1' configuration
   Verifying Hash Integrity ... sha256,rsa2048:dev+ OK
   Trying 'kernel@1' kernel subimage
     Description:  compressed kernel
     Created:      2020-10-15  15:44:52 UTC
     Type:         Kernel Image
     Compression:  lzma compressed
     Data Start:   0xa20000dc
     Data Size:    2620262 Bytes = 2.5 MiB
     Architecture: AArch64
     OS:           Linux
     Load Address: 0x80080000
     Load Size:    unavailable
     Entry Point:  0x80080000
     Hash algo:    sha256
     Hash value:   2eee092ccbd0a45f7d73aa1086d28994a60a615b7e967f0508e956a776bfe2b0
   Verifying Hash Integrity ... sha256+ OK
## Loading ramdisk from FIT Image at a2000000 ...
   Using 'rev1_pre_production@1' configuration
   Trying 'ramdisk@1' ramdisk subimage
     Description:  compressed ramdisk
     Created:      2020-10-15  15:40:21 UTC
     Type:         RAMDisk Image
     Compression:  lzma compressed
     Data Start:   0xa23139d4
     Data Size:    5474268 Bytes = 5.2 MiB
     Architecture: AArch64
     OS:           Linux
     Load Address: 0xb0000000
     Load Size:    unavailable
     Entry Point:  0xb0000000
     Hash algo:    sha256
     Hash value:   1bece01a0ad3c63006f24f6a07cad6f3ff023469907c202c8fc07eb5fda057b2
   Verifying Hash Integrity ... sha256+ OK
   Loading ramdisk from 0xa23139d4 to 0xb0000000
## Loading fdt from FIT Image at a2000000 ...
   Using 'rev1_pre_production@1' configuration
   Trying 'rev1_pre_production_fdt@1' fdt subimage
     Description:  rev1 pre production device tree
     Created:      2020-10-15  15:40:21 UTC
     Type:         Flat Device Tree
     Compression:  uncompressed
     Data Start:   0xa22d8650
     Data Size:    60328 Bytes = 58.9 KiB
     Architecture: AArch64
     Hash algo:    sha256
     Hash value:   b30932fd6e42020720ae6a062da9df0a5b5049f26b1bd8ddafd90cb9759a2922
   Verifying Hash Integrity ... sha256+ OK
   Loading fdt from 0xa22d8650 to 0x8f000000
   Booting using the fdt blob at 0x8f000000
   Uncompressing Kernel Image ... OK
   Loading Ramdisk to 8fac7000, end 8ffff7dc ... OK
   Loading Device Tree to 000000008fab5000, end 000000008fac6ba7 ... OK
WARNING: ethact is not set. Not including ethprime in /chosen.

======================================
= Galaxy Gateway Network Information =
======================================

WARNING: The deep space observatory is offline HTB{
INFO: Communication systems are offline reference code: 547311173_
WARNING: Unauthorized subroutines detected! reference code: n37w02k_
WARNING: The satellite dish can not sync with the swarm. reference code: c0mp20m153d}

             ______
          ,'"       "-._
        ,'              "-._ _._
        ;              __,-'/   |
       ;|           ,-' _,'"'._,.
       |:            _,'      |\ `.
       : \       _,-'         | \  `.
        \ \   ,-'             |  \   \
         \ '.         .-.     |       \
          \  \         "      |        :
           `. `.              |        |
             `. "-._          |        ;
             / |`._ `-._      L       /
            /  | \ `._   "-.___    _,'
           /   |  \_.-"-.___   """"
           \   :            /"""
            `._\_       __.'_
       __,--''_ ' "--'''' \_  `-._
 __,--'     .' /_  |   __. `-._   `-._
<            `.  `-.-''  __,-'     _,-'
 `.            `.   _,-'"      _,-'
   `.            ''"       _,-'
     `.                _,-'
       `.          _,-'
         `.   __,'"
           `'"

Starting kernel ...

Development login enabled: no
Debugging mode enabled: yes
Safe mode enabled: no

SATDish - Managment User Terminal.
Dish is offline
admin login:


```

</details>

The flag is then split into four parts right above the ASCII image of a satellite, concatenating them gave me the flag:

`HTB{547311173_n37w02k_c0mp20m153d}`