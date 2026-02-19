d---
title: "Arch Linux 使用笔记"
date: 2025-05-02 20:38:57
tags:
---

## Arch Linux 使用笔记

### 1.Steam游戏使用NVIDIA显卡启动游戏

需要在启动参数里加入如下参数：

```
__NV_PRIME_RENDER_OFFLOAD=0 __GLX_VENDOR_LIBRARY_NAME=nvidia %command%
```

注意0还是1取决于独立显卡是GPU0还是GPO1,以上内容来自arch linux PRIME 相关内容。

### 2.bluetooth

安装bluez bluez-utils,提供bluetoothctl功能

```bash
sudo pacman -S bluez bluez-utils
```

启用btusb

```bash
modprobe btusb
```

启用bluetooth并启动：

```bash
sudo systemctl enable bluetooth
sudo systemctl start bluetooth
```

### 3.os-prober

注意第一步编辑/etc/default/grub 文件,启用探测其他操作系统

```
GRUB_DISABLE_OS_PROBER=false
```

使用os-prober探测windows启动项

```bash
sudo os-prober
```

生成grub配置文件

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

### 4.fstab

在arch linux直接使用genfstab需要安装arch-install-scripts

```bash
sudo pacman -S arch-install-scirpts
```

生成fstab文件

```bash
sudo genfstab /
```

当然也可以直接修改/etc/fstab文件，我也推荐这么做
以下是fstab文件例子：

```
# UUID=4265f350-0ffa-4474-8bf8-b0dc9812ead9
/dev/nvme0n1p8          /               ext4            rw,relatime     0 1

# UUID=2C7D-1124
/dev/nvme0n1p6          /boot           vfat            rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,utf8,errors=remount-ro 0 2

# UUID=A62E-F384 LABEL=SYSTEM
/dev/nvme0n1p1          /boot2          vfat            rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,utf8,errors=remount-ro 0 2

# UUID=80d6aa7e-5b55-4c61-ac91-87df704f79a0
/dev/nvme0n1p7          none            swap            defaults        0 0

# UUID=10A70B5E10A70B5E
/dev/nvme0n1p3		/mnt/c_driver	ntfs-3g		defaults	0 0

# UUID=D850FD3A50FD2044
/dev/sda2		/mnt/HDD	ntfs-3g		defaults	0 0

# UUID=186a2a5a-ee28-a640-9a39-78467b6f2f49
/dev/sda3		/mnt/HDD2	ext4		defaults	0 0
```

注意：要挂在NTFS文件系统的磁盘需要安装ntfs-3g

```bash
sudo pacman -S ntfs-3g
```

然后编写/etc/fstab文件，每个挂载磁盘前面加上UUID,查询UUID的方法：
可以先使用lsblk查看所有的磁盘分区及挂在情况

```bash
lsblk
```

再查询指定分区UUID

```bash
sudo blkid /dev/nvme0n1p1
```

然后按照fstab文件格式将要挂在的分区写入后重启即可自动挂载。

### 5.关于更新arch

```bash
sudo pacman -S archlinux-keyring archlinuxcn-keyring
sudo pacman -Syu
```

### 6.openssh

安装openssh

```
sudo pacman -S openssh
```

查看openssh状态

```
sudo systemctl status sshd
```

如果 Open SSH 服务器未运行，终端应显示“inactive”。如果是这种情况，您可以通过输入以下命令来运行 Open SSH：

```
sudo systemctl start sshd
```

然后，通过再次输入以下内容来确保它是否正在运行：

```
sudo systemctl status sshd
```

如果 Open SSH 正在运行，提示符将以绿色显示“active”。如果要终止 SSH 服务器，请输入以下内容：

```
sudo systemctl stop sshd
```

### 7.vim剪切板与系统剪切板互通

查看vim是否支持与系统剪切板互通，可以用这个来检查clipboard那一项是 + 还是 - ：

```bash
vim --version | grep clipboard
```

如果结果里出现-clipboard ，可以卸载vim安装gvim

```bash
sudo pacman -R vim
sudo pacman -S gvim
```

安装后再查看vim的剪切板特性，看到 +clipboard，表示支持与系统剪切板互通
然后不管你是 `set clipboard = unnamed` 还是用寄存器 "+p 和 "+y，应该都可以了

