# archlinux 安装注意事项
0 硬盘要GPT格式

1 分区
~~~
fdisk /dev/sda
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
mount /dev/sda2 /mnt
mkdir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
~~~

2 装字体，中文

~~~
pacman -S noto-fonts-cjk noto-fonts noto-fonts-emoji
sudo vim /etc/locale.gen
sudo locale-gen
export LANG=zh_CN.utf8
~~~

3 iwctl 无线网卡，网络sig别有特殊的字符
~~~
~~~

4 Arch Linux CN的软件源地址。**注意server的=两边有空格**
~~~
sudo vim /etc/pacman.conf
~~~
添加如下，并把multilib打开
~~~
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
~~~
保存后，更新软件源并安装密钥环：

~~~
sudo pacman -Sy //更新源 -Syu是更新系统本身
sudo pacman -S yay //装yay
sudo pacman -S archlinuxcn-keyring
sudo pacman -S firefox  google-chrome
~~~
~~~

2
~~~
~~~
5 建立启动项
~~~
sudo pacman -S os-prober ntfs-3g efibootmgr
sudo grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Grub
sudo xed /etc/default/grub
sudo grub-mkconfig -o /boot/grub/grub.cfg
~~~
没win启动项，解决方法是 vim /etc/default/grub 加入一行 GRUB_DISABLE_OS_PROBER="false"
保存后执行 
~~~
grub-mkconfig -o /boot/grub/grub.cfg
~~~


6 nvidia 驱动
https://howto.lintel.in/install-nvidia-arch-linux/

7 高分屏调节字体Xfce下

设置 外观 字体 DPI 280

设置 桌面  图标 图标大小 96

xrandr -s 0   默认分辨率 

每个面板调节大小。基本X2