# 使用方法

1. 下载 : `git clone https://github.com/cattyhouse/new-uboot-for-N1 && cd new-uboot-for-N1`

1. 复制 : 把本目录所有文件复制到 /boot, 遇到有重复的, 就覆盖. `sudo cp -rf * /boot/`

1. 找UUID : 找出 根目录的 的 UUID, 命令: `findmnt -n -o UUID -M /`

1. 设置UUID: 修改 `/boot/extlinux/extlinux.conf`, 将`root_uuid` 替换为上面找到的UUID

1. 重启, 就可以用新的uboot启动当前的内核了.

# 其他补充

1. 我编译的内核下载地址: https://kr1.us.to/kernel/

1. 安装新的内核 比如 

`pacman -U linux-phicomm-n1-5.15.8-1-aarch64.pkg.tar.zst linux-phicomm-n1-headers-5.15.8-1-aarch64.pkg.tar.zst`

3. 注意, 我这个内核只能用这个 uboot 启动, n1 原装的 uboot 无法启动. 


# **额外补充**

当插入 2 个 USB 设备的时候, USB 启动变得随机, 原因是:

运行 strings /boot/u-boot.ext | grep -E 'boot_targets|bootcmd_usb' 得到

```
boot_targets=usb0 romusb mmc0 mmc1 mmc2 pxe dhcp
bootcmd_usb0=devnum=0; run usb_boot
```
可见, 这个 u-boot.ext 只会寻找 usb0


那么我做了以下改动

1. 用 `hexedit` 编辑 `u-boot.ext`, 做了如下改动

`boot_targets=usb0 romusb mmc0 mmc1 mmc2 pxe dhcp` 

改为

`boot_targets=usb0 usb1 romusb mmc0 mmc1 mmc2 pxe`

-------------
`bootcmd_mmc0=devnum=0; run mmc_boot` 

改为

`bootcmd_usb1=devnum=1; run usb_boot`

> 注意 hexedit 改动 二进制文件, 不能增加任何 bit 也不能删除任何 bit, 只能一对一替换, 因为 mmc0 是 sd card, 根本不需要, 可以将它改为 usb1

改完之后, 得到的是这样的内容

运行 strings /boot/u-boot.ext | grep -E 'boot_targets|bootcmd_usb'

```
boot_targets=usb0 usb1 romusb mmc0 mmc1 mmc2 pxe
bootcmd_usb1=devnum=1; run usb_boot
bootcmd_usb0=devnum=0; run usb_boot
```

2. 还要改动 `s905_autoscript`

在原来的基础上增加一个

```
if fatload usb 1 0x1000000 u-boot.ext; then go 0x1000000; fi;
```

