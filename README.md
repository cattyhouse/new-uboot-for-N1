# 使用方法

1. 下载 : `git clone https://github.com/cattyhouse/new-uboot-for-N1 && cd new-uboot-for-N1`

1. 复制 : 把本目录所有文件复制到 /boot, 遇到有重复的, 就覆盖. `sudo cp -rf * /boot/`

1. 找UUID : 找出 根目录的 的 UUID, 命令: `findmnt -n -o UUID -M /`

1. 设置UUID: 修改 `/boot/extlinux/extlinux.conf`, 将`replace_me_with_root_uuid` 替换为上面找到的UUID

1. 检查 /boot/extlinux/extlinux.conf 所有项目, 确保每一行都设置了正确的路径和文件名

1. reboot
