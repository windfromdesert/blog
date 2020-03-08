##  DELL笔记本重新安装archlinux+gnome记录

首先安装 [archlinux][1] ，接下来主要记录安装 Gnome 桌面环境。

Xorg：首先是装Xorg

	pacman -S xorg xorg-server xorg-xinit

安装触摸板驱动：

	pacman -S xf86-input-synaptics

安装显卡驱动：

	pacman -S xf86-video-intel

安装 Gnome 桌面系统：

	pacman -S gnome gnome-extra gnome-tweak-tool

	> 注：gnome-extra包可以提供额外的常用软件和几个游戏，你可以安装时选择你要的软件，没有必要全选，当然也可以不装这个包。

然后安装gdm登录管理器：

	pacman -S gnome gdm

将 gdm 设置为开机自启动，这样开机时会自动载入桌面

	systemctl enable gdm

最后，你还应该建立一个普通新用户用于登录Gnome

	useradd -m  maxzhao # 例如建立一个名叫 maxzhao 的用户
	passwd maxzhao # 设置用户密码
	vim  /etc/sudoers # 编辑 sudo权限

	在 `root ALL=(ALL) ALL` 下面添加 
	maxzhao ALL=(ALL) ALL

最后别忘了安装网络，否则重启后会没有网络。

	pacman -S dhcp dhcpcd  net-tools  NetworkManager 
	pacman -S iw wpa_supplicant  # 无线
	pacman -S dialog netctl # 登录系统后可以使用 `sudo wifi-menu` 连接 WIFI 网络


2020年3月8日

[1]:	https://github.com/windfromdesert/blog/blob/master/2017/archlinux%E5%AE%89%E8%A3%85.md
 
