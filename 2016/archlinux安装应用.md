## [[Archlinux]] 我的安装应用

+   slock
    -   非常简单的屏幕锁定应用，直接 pacman -S slock 安装即可
    -   安装后只要在终端输入 slock 即可锁定屏幕
    -   锁定屏幕后碰触键盘或鼠标会发出声音并显示全色仍然锁定屏幕
    -   用键盘输入登录密码后，屏幕即解除锁定。

+   我的无线网卡 intel Wireless-AC 8260 安装过程中出现问题，无法连接网络，百试无解，最后在 [这里][1] 找到一段命令，很幸运地竟然解决了问题，特在此记录。命令如下：

        > sudo rmmod iwlmvm iwlwifi && sudo modprobe iwlmvm iwlwifi

+   关闭默认的屏幕保护设置

    安装 archlinux 后，系统默认会在10分钟后关闭屏幕，有时候需要修改这个设置，比如关闭屏幕保护或者设置一个较长的时间。

    设置方法是对 [DPMS][2] 进行设置，DPMS的设置文件一般是在：`/etc/X11/xorg.conf.d/10-monitor.conf`，内容举例：

        Section "Monitor"
            Identifier "LVDS0"
            Option "DPMS" "true"   // 这里要设置为'true'
        EndSection

        Section "ServerLayout"
            Identifier "ServerLayout0"
            Option "StandbyTime" "0"    // 在这里设置待机时间，时间单位为分钟。
            Option "SuspendTime" "0"    // 在这里设置休眠时间
            Option "OffTime"     "0"    // 在这里设置关闭屏幕时间
            Option "BlankTime"   "0"    // 在这里设置屏幕保护时间
        EndSection        
        
+   配置系统快捷键，给截屏工具 scrot 设置全屏快捷键。

    fluxbox 桌面管理器的快捷键设置文件在：`$ cd ~/.fluxbox/keys`

    在 keys 文件尾部添加以下文字：

        Control Mod1 f :exec scrot desktop.jpg -e 'mv $f ~/'

    以上这行命令的意思是设置快捷键 Control 即 `<ctrl>` 与 Mod1 即 `<Alt>` 与 f 组合键，即 `<Ctrl>+<Alt>+f` 。执行命令为：exec scrot desktop.jpg -e 'mv $f ~/'。其中 exec 代表执行命令，scrot desktop.jpg 代表将截屏保存为 desktop.jpg 文件，-e 'mv $f ~/' 代表将 desktop.jpg 保存在 ~/ 目录中，其中 mv 是移动文件，$f 指代 desktop.jpg 文件。


[1]: https://forum.manjaro.org/t/problems-getting-network-address-via-wireless/5007/2 "Problems getting network address via wireless"
[2]: https://wiki.archlinux.org/index.php/Display_Power_Management_Signaling "Display Power Management Signaling"
