# archlinux 安装注意事项
1 设置 外观 字体 DPI 280
2 设置 桌面  图标 图标大小 96
3 xrandr -s 0   默认分辨率 
4 每个面板调节大小。基本X2

~~~
sudo grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Grub
sudo xed /etc/default/grub
sudo grub-mkconfig -o /boot/grub/grub.cfg
~~~

