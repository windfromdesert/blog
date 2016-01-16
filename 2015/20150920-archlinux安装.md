Archlinux 系统基本安装见上一篇，这里主要记录一些增强型的系统实现。

### 字体安装

+   复制字体文件夹到 /usr/share/fonts/ 目录下
+   接着在命令行下输入以下命令（比如，这里创建了一个TTC字体文件夹）

        #cd /usr/share/fonts/TTC
        #mkfontscale
        #mkfontdir       

+   更新字体缓存： fc-cache -fv

### fluxbox 中文字体设置

+   在转换主题样式 style 的过程中，会发现有时候 标题栏或者工具栏的中文显示为方框，那是因为 fluxbox 中的字体设置问题。
+   一次性解决的办法是编辑 ~/.fluxbox/overlay 文件。

        # 菜单标题的字体
        menu.title.font: simsun-10
        # 菜单组标题的对齐方式
        menu.title.justify: center
        # 菜单项目的字体
        menu.frame.font: simsun-10
        # 菜单项目的对齐方式
        menu.frame.justify: left
        # 窗口标题栏文字的字体
        window.font: simsun-10
        # 窗口标题栏文字的对齐方式
        window.justify: center
        # 窗口标题聚焦时的背景颜色|5/5/f
        window.label.focus.color: rgb:4e/8f/cf
        window.label.focus.colorto: rgb:4e/8f/cf
        # 时钟的字体
        toolbar.clock.font: simsun-10
        # 工作区名称的字体
        #toolbar.workspace.font: simsun
        # 图标栏的字体
        toolbar.iconbar.focused.font: simsun-10
        toolbar.iconbar.unfocused.font: simsun-10

+   把其中的字体改成自己系统中的已安装中文字体，查看系统字体命令： fc-list
+   字体名称后面-11为字体大小，再后面跟:bold表示粗体显示，中间都没有空格。

### fluxbox 壁纸的实现

+   安装 feh （可以从arch库直接安装）
+   在终端中输入命令： fbsetbg 壁纸文件路径。将看到桌面壁纸实时更新。如：
        
        fbsetbg ~/image/111.jpg

+   在 ~/.fluxbox/init 文件中添加以下命令：
        
        session.screen0.rootCommand: fbsetbg -l

### linux 系统时间

+   linux 分为硬件时间与系统时间。
+   查看当前的硬件时间与系统时间设置

        timedatectl

+   把硬件时间调整到本地时区

        timedatectl set-local-rtc 1

+   把硬件时间调回UTC

        timedatectl set-local-rtc 0

### 安装中文输入法 fcitx.

可以在 arch 库中直接安装： fcitx fcitx-im fcitx-qt5 fcitx-table-extra fcitx-table-other

在 ~/.fluxbox/startup 中加入以下：

    fcitx
    export GTK_IM_MODULE=fcitx
    export QT_IM_MODULE=fcitx
    export XMODIFIERS="@im=fcitx"

+   fcitx 有一个很酷的功能。就是快捷键 ctrl + ; 可以在不同的程序间进行复制与粘贴。有时候这是一个非常有用的特性。

### fluxbox 状态栏的时间显示格式

右键点击桌面，fluxbox menu --> configure --> 工具栏 --> 编辑时间显示格式

    %m-%d,%A,%k:%M

### 安装 irssi

+   直接从 arch 库中安装 irssi

        sudo pacman -S irssi

+   安装提醒插件 beep_beep.pl
    -   进入 [irssi scripts][1] 官网。
    -   下载 beep_beep.pl 文件，然后将文件放在：~/.irssi/scripts/ 目录下，如果没有此目录可以自行创建。
    -   如果要设置启动时自动加载插件，就将此插件也放入目录：~/.irssi/scripts/autorun/ ，或者在 autorun 文件夹中创建一个插件的软链接。
    -   这个提醒插件未带提醒声音的音频文件，所以需先在网上找一个自己喜欢的提醒音频文件，比如 alarm.wav，并将此音频文件放入：~/.irssi/scripts
    -   编辑 beep_beep.pl 文件，在文件中找到这一行：

            Irssi::settings_add_str("lookandfeel", "beep_cmd", "play ~/.irssi/scripts/beep_beep.wav > /dev/null &");
            
    -   修改为：(将 play 改为 aplay 是为了确保linux默认可以播放音频文件；添加 -q 参数是为了在提醒时 irssi 界面内不显示播放路径信息)

            Irssi::settings_add_str("lookandfeel", "beep_cmd", "aplay -q ~/.irssi/scripts/beep_beep.wav > /dev/null &");

### 改善 xterm 的复制粘贴

+   默认 xterm 选中即复制，粘贴是使用快捷键 shift + insert
+   而且 xterm 复制后不是放在系统剪贴板，而是放在选中缓冲中，所以默认时无法将 xterm 中的命令粘贴到其它地方。
+   但可以通过以下方法改善：
    -   修改 ~/.Xresources 文件，在开头加入以下几句，可让 xterm 的复制进入剪贴板，不再过选中缓冲。
        
            *VT100*translations: #override \n\ 
            Shift <KeyPress> Insert:insert-selection(CLIPBOARD, CUT_BUFFER1) \n\ 
            ~Shift~Ctrl<Btn2Up>: insert-selection(CLIPBOARD, CUT_BUFFER1) \n\ 
            ~Shift<BtnUp>: select-end(CLIPBOARD, CUT_BUFFER1) 

### 调节 xterm 行距

+   修改 ~/.Xresources 文件，添加以下代码：

        XTerm*scaleHeight: 1.2

+   行距的大小只要调整后面的数字即可。取值从0.9-1.5

### 设置代理

+   安装 [shadowsocks] [1]

    直接在 arch 库中安装 shadowsocks 即可。

+   设置 /etc/shadowsocks/config.json 

        {
	        "server":"remote-shadowsocks-server-ip-addr",
	        "server_port":443,
        	"local_address":"127.0.0.1",
	        "local_port":1080,
	        "password":"your-passwd",
	        "timeout":300,
	        "method":"aes-256-cfb",
	        "fast_open":false,
	        "workers":1
        }

+   以守护进程形式运行客户端
+   Shadowsocks的systemd服务可在/etc/shadowsocks/里调用不同的conf-file.json（以conf-file为区分标志），例： 在/etc/shadowsocks/中创建了foo.json配置文件，那么执行以下语句就可以调用该配置：

        # systemctl start shadowsocks@foo

+   若需开机自启动，则：

        # systemctl enable shadowsocks@foo

+   提示: 可用journalctl -u shadowsocks@foo来查询日志；

+   使用 cow 将 shadowsocks sock5 代理转为 http 代理，让 xterm shell 可以使用代理更新某新被封的源。
    -   从 AUR 中安装 [cow] [2]
    -   按软件说明设置 cow 配置文件
    -   启动 cow proxy
        
            sudo systemctl start cow@user

    -   在 ~/.bashrc 中加入以下几行：(假设我将代理端口设置为8888)

            export http_proxy=http://127.0.0.1:8888/
            export https_proxy=$http_proxy
            export ftp_proxy=$http_proxy
            export rsync_proxy=$http_proxy

    -   在终端中输入以下命令，使 .bashrc 设置立即生效。

            source ~/.bashrc

    -   不要使用全局代理时，将上述 .bashrc 中的几行代码用 # 注释掉即可。
    -   同时停用 cow 代理服务

            sudo systemctl stop cow@user

### chromium 因未安装插件而无法播放视频问题

+   只须安装插件 [chromium-pepper-flash] [3] 即可。

### 连接 android 移动设备

+   直接从 arch 库中安装 android-tools, android-udev
+   将自己的用户名加入到 adbusers 组中

        gpasswd -a username adbusers

+   将移动设备的 USB 调试选项打开
+   连接移动设备与PC电脑，在手机端可能会需要认证一下。点一下 OK 即可。
+   然后就可以使用 adb shell, adb push, adb pull 来操作了。

### 安装屏幕截图程序 scrot

+   直接从 arch 库中安装

        sudo pacman -S scrot

+   scrot 一般用法

        scrot desktop.png // 抓取整个桌面
        scrot -bs window.png // 选项 b 使抓取窗口时将外边框抓取下来，选项 s 让用户选择抓取哪个窗口
        scrot -s test1.png // 执行此命令后，使用鼠标拖曳的矩形区域进行抓取。

### PDF 阅读器的选择与安装

+   我选择的是 zathura 阅读器
    -   在 archlinux 库中的地址在 [这里] [4]
    -   按照页面上的说明，我选择了最小安装（只用来阅读PDF文件）
        
            sudo pacman -S zathura zathura-pdf-poppler poppler-data 

    -   安装完后，可以在命令行直接键入 zathura 启动阅读器
    -   使用方法与 vim 类似，可以用冒号后加命令的形式
    -   配置文件在 ~/.config/zathura/zathurarc 中设置， [我的配置文件] [5]
    -   快捷键与 VIM 相同
    -   在主界面按 TAB 键可以跳转到目录界面。

### 显示器校色软件 dispcalGUI 的安装与使用

+   可以直接在库中安装：sudo pacman -S dispcalgui argyllcms gksu
+   然后启动: sudo dispcalGUI
+   启动后需激活硬件 spyder2，需用到CD光盘或者 [Spyder2express_2.3.6_Setup.exe] [6]
+   调整时要关闭显示器屏幕保护，Xorg 的 DPMS (Display Power Management Signaling) 设置文件是：/etc/X11/xorg.conf.d/10-monitor.conf，内容象这样：

        Section "Monitor"
            Identifier "LVDS0"
            Option "DPMS" "false"
        EndSection

        Section "ServerLayout"
            Identifier "ServerLayout0"
            Option "StandbyTime" "0"
            Option "SuspendTime" "0"
            Option "OffTime" "0"
        EndSection
        
+   Note: If the "OffTime" option does not work replace it with the following, (change the "blanktime" to "0" to disable screen blanking)
+   当最终生成 icc 文件后，我选择不自动安装 icc 配置文件
+   因为我用的窗口管理器是 fluxbox，所以可以编辑文件： ~/.fluxbox/startup，增加以下代码
+   或者在 Xinitrc 中添加以下代码，Xinitrc 文件样本可以复制到用户目录中：cp /etc/X11/xinit/xinitrc ~/.xinitrc

        /usr/bin/dispwin -d0 /home/wangbin/iccprofile/AOC-ARCH-20151010.icc



[1]:    https://wiki.archlinux.org/index.php/Shadowsocks_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)     "shadowsocks_(简体中文)"
[2]:    https://aur.archlinux.org/packages/cow-proxy/       "cow-proxy"
[3]:    https://aur.archlinux.org/packages/chromium-pepper-flash/   "chromium-pepper-flash/"
[4]:    https://www.archlinux.org/packages/community/i686/zathura/ "zathura"
[5]:    https://github.com/windfromdesert/code/blob/master/zathurarc "zathurarc"
[6]:    http://pan.baidu.com/s/1pJ9YBX5 "spyder2express_2.3.6_setup.exe"
