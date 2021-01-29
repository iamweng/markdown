# Arch

## Install

```bash
# 查看控制台键盘布局列表,默认为 us
ls /usr/share/kbd/keymaps/**/*.map.gz
# 修改键盘布局并持续生效
vi /etc/vnconsle.conf

KEYMAP=de-latin1

# 验证主板引导模式,未找到文件说明为 bios 反之则为 uefi
ls /sys/firmware/efi/efivars
# 确认系统时间准确
timedatectl set-ntp true
# 检查 timedatectl 服务状态
timedatectl status
# 检查硬盘分区
fdisk -l
# 建立硬盘分区
## 如果为 bios with mbr, 则需要一个根分区挂载在 /mnt 下,和一个交换分区,格式如下
/mnt /dev/sda1 Linux <DISK_SIZE>
[swap] /dev/sda2 Linux swap <512M
## 如果为 uefi with gpt, 则需要在 bios with mbr 的基础上建立一个 efi 分区, 格式如下
/mnt/efi /dev/sda1 efi 260-512M
# 格式化交换分区
mkswap /dev/sda2
# 挂载交换分区
swapon /dev/sda2
# 格式化分区为 ext4 格式
mkfs.ext4 /dev/sda1
# 将根分区挂载到 /mnt
mount /dev/sda1 /mnt
# 配置镜像源
vi /etc/pacman.d/mirrorlist
# 使用 pacstrap 脚本安装 base 软件包和 linux 内核以及常规硬件固件
pacstrap /mnt base linux linux-firmware
# 生成 fstab 文件
genfstab -U /mnt >> /mnt/etc/fstab
# 切换到新安装的系统
arch-chroot /mnt
# 设置系统时区,在假定硬件时间已经被设置为 utc 时间
ln -sf /usr/share/zoneinfo/<REGION>/<CITY> /etc/localtime
# 运行 hwclock 生成 /etc/adjtime
hwclock --systohc
# 配置本地化信息
vi /etc/locale.gen
# 生成 locale 讯息
locale-gen
# 创建 hostname 文件并添加对应信息

vi /etc/hosts

127.0.0.1   localhost
::1         localhost

# 你通常不需要创建 initramfs,因为你在执行 pacstrap 是以及安装了 linux，这时 mkinitcpio 会被自动运行,对于 LVM, system encryption, RAID 修改 mkinitcpio.conf 并用一下命令重新创建一个 initramfs
mkinitcpio -P
# 设置 root 用户密码
passwd
# 安装 grub 引导系统
pacman -S dosfstools grub efibootmgr
## bios mbr 特殊操作
grub-install --target=i386-pc /dev/sda
## urfi gpt 特殊操作
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
#使用 grub-mkconfig 生成/boot/grub/grbu.cfg,每次添加或移除内核都需要运行grub-mkconfig
grub-mkconfig -o /boot/grub/grub.cfg
# 退出 chroot 环境
exit
# 重新启动系统
reboot
```