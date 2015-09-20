Archlinux 系统基本安装见上一篇，这里主要记录一些增强型的系统实现。

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

### fluxbox 状态栏的时间显示格式

右键点击桌面，fluxbox menu --> configure --> 工具栏 --> 编辑时间显示格式

    %m-%d,%A,%k:%M


