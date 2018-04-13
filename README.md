U-Boot 1.1.3 for MT7620 from WRTnode
==================

Boot log for release-20180412
-------------

```
U-Boot 1.1.3 (Apr 12 2018 - 21:17:47)

Board: Ralink APSoC DRAM:  64 MB
GPIO#44 updated GPIOMODE register: 001a311c -> 001ab11c
MT7620 gpio init : WPS / RESET pin
GPIO#2 updated GPIOMODE register: 001ab11c -> 001ab11d
relocate_code Pointer at: 83fac000
enable ephy clock...done. rf reg 29 = 5
SSC disabled.
spi_wait_nsec: 29
spi device id: ef 40 18 0 0 (40180000)
find flash: W25Q128FV
raspi_read: from:30000 len:1000
raspi_read: from:30000 len:1000
============================================
U-Boot_mt7620 Version: 0.0.0.1
--------------------------------------------
ASIC 7620_MP (Port5<->None)
DRAM component: 512 Mbits DDR, width 16
DRAM bus: 16 bit
Total memory: 64 MBytes
Flash component: SPI Flash
Date:Apr 12 2018  Time:21:17:47
============================================
icache: sets:512, ways:4, linesz:32 ,total:65536
dcache: sets:256, ways:4, linesz:32 ,total:32768

 ##### The CPU freq = 580 MHZ ####
 estimate memory size =64 Mbytes

Press press WPS button for more than 2 seconds to run web failsafe mode

WPS button is pressed for:  0 second(s)

Catution: WPS button wasn't pressed or not long enough!
Continuing normal boot...

raspi_read: from:40028 len:6
raspi_read: from:40000 len:80
Valid MAC address detected: 00:02:2a:07:7c:42


================================================================
Please choose the operation:
   0: System Load Linux then write to Flash via Serial.
   1: Load system code to SDRAM via TFTP.
   2: Load system code then write to Flash via TFTP.
   3: Boot system code via Flash (default).
   4: Entr boot command line interface.
   7: Load Boot Loader code then write to Flash via Serial.
   8: System Load UBoot to SDRAM via TFTP.
   9: Load Boot Loader code then write to Flash via TFTP.
================================================================

You choosed 4
                                                                              0

4: System Enter Boot Command Line Interface.

U-Boot 1.1.3 (Apr 12 2018 - 21:17:47)
MT7620 # printenv
bootcmd=run usbargs;usb start;fatload usb 0 0x80c00000 uimage;bootm 0x80c00000
usbargs=setenv bootargs root=8:2 rootdelay=5 rootfstype=ext4 rw eth=${ethaddr} console=ttyS0,${baudrate}
bootdelay=2
ipaddr=192.168.1.1
serverip=192.168.1.100
baudrate=115200
stdin=serial
stdout=serial
stderr=serial
ethaddr=00:02:2a:07:7c:42

Environment size: 321/4092 bytes
MT7620 #
```


Prerequisites
-------------

Get Ralink SDK, filename: MTK_Ralink_ApSoC_SDK_4120_20120607.tar.bz2.

Prepare Linux develop enviroment which must be 32-bit for MTK_Ralink_ApSoC_SDK buildroot-gcc342 are all 32-bit. For example:
```linux
$ file /opt/buildroot-gcc342/bin/mipsel-linux-uclibc-gcc
/opt/buildroot-gcc342/bin/mipsel-linux-uclibc-gcc: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.4.1, stripped
```

Unpack the "toolchain/buildroot-gcc342" folder to "/opt/buildroot-gcc342", or wherever you want, but then you need to replace the paths below.


Building
--------

	$ git clone git@github.com:agustim/u-boot_wrtnode.git
	$ export CONFIG_CROSS_COMPILER_PATH=/opt/buildroot-gcc342/bin  # necessary if not in /opt
	$ export PATH=/opt/buildroot-gcc342/bin:$PATH
	$ cp wrtnode_config .config
	$ make menuconfig # Exit and save 
	$ make
	$ hexdump -C uboot.bin | head  # look at first bytes to verify

NOTE: The output filename for directly flashing the binary has to be **uboot.bin**.


Reference first bytes
---------------------

The first bytes of uboot.bin are some sort of interrupt vector table and should look approximately like this (exact address entries may vary):

```
00000000  ff 00 00 10 00 00 00 00  fd 00 00 10 00 00 00 00  |................|
00000010  7f 02 00 10 00 00 00 00  7d 02 00 10 00 00 00 00  |........}.......|
00000020  7b 02 00 10 00 00 00 00  79 02 00 10 00 00 00 00  |{.......y.......|
00000030  77 02 00 10 00 00 00 00  75 02 00 10 00 00 00 00  |w.......u.......|
00000040  73 02 00 10 00 00 00 00  71 02 00 10 00 00 00 00  |s.......q.......|
00000050  6f 02 00 10 00 00 00 00  6d 02 00 10 00 00 00 00  |o.......m.......|
00000060  6b 02 00 10 00 00 00 00  69 02 00 10 00 00 00 00  |k.......i.......|
00000070  67 02 00 10 00 00 00 00  65 02 00 10 00 00 00 00  |g.......e.......|
00000080  63 02 00 10 00 00 00 00  61 02 00 10 00 00 00 00  |c.......a.......|
00000090  5f 02 00 10 00 00 00 00  5d 02 00 10 00 00 00 00  |_.......].......|
000000a0  5b 02 00 10 00 00 00 00  59 02 00 10 00 00 00 00  |[.......Y.......|
000000b0  57 02 00 10 00 00 00 00  55 02 00 10 00 00 00 00  |W.......U.......|
000000c0  53 02 00 10 00 00 00 00  51 02 00 10 00 00 00 00  |S.......Q.......|
000000d0  4f 02 00 10 00 00 00 00  4d 02 00 10 00 00 00 00  |O.......M.......|
000000e0  4b 02 00 10 00 00 00 00  49 02 00 10 00 00 00 00  |K.......I.......|
000000f0  47 02 00 10 00 00 00 00  45 02 00 10 00 00 00 00  |G.......E.......|
00000100  43 02 00 10 00 00 00 00  41 02 00 10 00 00 00 00  |C.......A.......|
00000110  3f 02 00 10 00 00 00 00  3d 02 00 10 00 00 00 00  |?.......=.......|
00000120  3b 02 00 10 00 00 00 00  39 02 00 10 00 00 00 00  |;.......9.......|
00000130  37 02 00 10 00 00 00 00  35 02 00 10 00 00 00 00  |7.......5.......|
00000140  33 02 00 10 00 00 00 00  31 02 00 10 00 00 00 00  |3.......1.......|
00000150  2f 02 00 10 00 00 00 00  2d 02 00 10 00 00 00 00  |/.......-.......|
00000160  2b 02 00 10 00 00 00 00  29 02 00 10 00 00 00 00  |+.......).......|
```
