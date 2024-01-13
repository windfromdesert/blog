### [[Archlinux]] 系统音乐格式转换与编辑

#### 音乐编辑软件 ardour

+   直接在官方库中安装即可： sudo pacman -S ardour
+   安装后在命令行输入 ardour4 即可启动程度，方便起见，可以加入 fluxbox 的右键启动菜单。

#### 将音频转换为 320k mp3

一般音乐源文件是 wav、flac、ape中的一种， wav 不必说，如果是 flac，则：

    sudo pacman -S flac //这一步为安装 flac 编解码器
    flac -o [输出文件名.wav] -d [输入文件名.flac] //这一步是将 flac 音乐转换为 wav格式
    flac -8 -o 输出档案路径 输入档案路径 // 将wav格式压缩为flac，-8 是压缩等级，数字从0-8，数字越大压得越小
    sudo pacman -S mac //这一步是安装 ape 格式的编解码器
    mac [ape输入文件] [WAV输出文件] -d //这一步将APE文件转换为WAV格式
    sudo pacman -S lame //这一步为安装 mp3 lame 编解码器
    lame --preset insane [输入WAV文件] [输出文件名.mp3] //这一步使用 lame 的预置 insane 最高品质参数转换为320k的mp3音乐文件
    sudo pacman -S ffmpeg //这一步是安装 ffmpeg 编解码器，它支持 aac 音频编码
    ffmpeg -i [输入WAV文件] -strict -2 -b:a 512k [输出的m4a格式文件] //其中 -strict -2 在新版 ffmpeg 中已不需使用

#### 音乐文件的 tag 编辑

直接从官方源安装 puddletag ： sudo pacman -S puddletag 即可。
