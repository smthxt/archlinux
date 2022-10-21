# archlinux 安装注意事项
官方文档
~~~
https://wiki.archlinux.org/title/Installation_guide#Partition_the_disks
~~~
0 硬盘要GPT格式，MBR转GPT有可能出错，硬盘得备份。

1 分区等基础命令命令

~~~
sudo fdisk -l
df -h 
lsblk   
~~~
可以先手动分区，再用脚本安装
也可以直接用脚本对整个硬盘操作。
~~~
fdisk /dev/sda

mkfs.fat -F32 /dev/sda1 
mkfs.ext4 /dev/sda2
mount /dev/sda2 /mnt //用不上

mkdir -p /mnt/boot/efi    //用不上
mount /dev/sda1 /mnt/boot/efi //用不上
~~~

2 archinstall脚本安装

~~~
sudo pacman -Syy archinstall
archinstall
~~~
屏幕如果字太小，可以用setfont调节一下，字体在/usr/share/kbd/consolefonts/下面

archinstall里面分区和指定 /boot /是关键。
非脚本方式，没有用过。有人成功了。如下操作

分区以后
~~~
mount /dev/sda2 /mnt
mkdir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
~~~
挂载好分区以后执行如下脚本

kde桌面环境脚本：
~~~
curl -O https://gitee.com/zhaocaiall/ArchInstall/raw/master/kde.sh
sh ./kde.sh
~~~
gnome桌面环境脚本：
~~~
curl -O https://gitee.com/zhaocaiall/ArchInstall/raw/master/gnome.sh
sh ./gnome.sh
~~~
deepin桌面环境脚本：
~~~
curl -O https://gitee.com/zhaocaiall/ArchInstall/raw/master/deepin.sh

sh ./deepin.sh
~~~

2 装字体，中文输入法

~~~
pacman -S noto-fonts-cjk noto-fonts noto-fonts-emoji
sudo vim /etc/locale.gen
sudo locale-gen
export LANG=zh_CN.utf8
~~~
重启
~~~
sudo pacman -S fcitx5-im fcitx5-chinese-addons fcitx5-rime
~~~
/etc/environment中添加如下内容:
~~~
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
~~~
重启

3 iwctl 无线网卡，网络sig别有特殊的字符
~~~
~~~

4 Arch Linux CN的软件源地址。*注意server的=两边有空格。*
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

5 建立启动项
~~~
sudo pacman -S os-prober ntfs-3g efibootmgr
sudo grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Grub
sudo xed /etc/default/grub
sudo grub-mkconfig -o /boot/grub/grub.cfg
~~~
没win启动项，解决方法是 vim /etc/default/grub 加入一行，或者取消注释 
~~~
GRUB_DISABLE_OS_PROBER="false"
~~~
保存后执行
~~~
grub-mkconfig -o /boot/grub/grub.cfg
~~~

6 nvidia 驱动

https://howto.lintel.in/install-nvidia-arch-linux/

7 高分屏调节字体Xfce下

    7.1设置 外观 字体 DPI 280

    7.2 设置 桌面  图标 图标大小 96

    7.3 xrandr -s 0   默认分辨率 

    7.4 每个面板调节大小。基本X2