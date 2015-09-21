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
