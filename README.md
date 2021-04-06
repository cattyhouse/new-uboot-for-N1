# 使用方法

1. 下载 : `git clone https://github.com/cattyhouse/new-uboot-for-N1 && cd new-uboot-for-N1`

1. 复制 : 把本目录所有文件复制到 /boot, 遇到有重复的, 就覆盖. `sudo cp -af * /boot/`

1. 找UUID : 找出 根目录的 的 UUID, 命令: `lsblk -f | grep -w '/' | xargs | cut -d ' ' -f4`

1. 设置UUID: 修改 `/boot/extlinux/extlinux.conf`, 将`根目录的标识符` 替换为上面找到的UUID

1. 设置网卡地址 : 随便在线生成一个网卡地址, 分隔符形式为冒号 (:), 然后编辑 `/etc/systemd/network/20-wired.network` (有线网卡的配置文件), 在[Link] 下方添加一行 MACAddress=`网卡地址`, 修改后这个文件最后的这部分看起来是这样的
    ````
    [Link]
    MACAddress=网卡地址
    ## set to yes to disable this network
    Unmanaged=no

    ````
    >注意: 前提是用的 systemd-networkd 配置的网络, 至于其他的方式配置的自行搜索如何在开机的时候设置网卡地址

1. 重启, 就可以用新的uboot启动当前的内核了.

# 其他补充

1. 我编译的内核下载地址: https://kr1.us.to/kernel/

1. 安装新的内核 `pacman -U linux-phicomm-n1-5.10.25-1-aarch64.pkg.tar.xz linux-phicomm-n1-headers-5.10.25-1-aarch64.pkg.tar.xz` , 然后重启.

1. 注意, 我这个内核只能用这个 uboot 启动, n1 原装的 uboot 无法启动. 

