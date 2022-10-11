### upx 权限修复工具

升级到最新的macOS Big Sur后，有一些软件出现无法打开的情况，提示如下【您没有权限来打开应用程序“XXX”。】，安装upx插件可以解决该问题。

1. 下载upx权限修复工具：https://www.macapp.so/upx/ 并安装upx；
2. 将打不开的软件拖到应用程序目录，在软件图标上右键，选择“显示包内容”，进入 Contents - MacOS 目录；
3. 打开终端，拷贝命令：sudo /Applications/upx -d   （注意最后有一个空格）粘贴到终端，然后将第2步的MacOS目录内的图标拖到终端，得到类似命令：sudo /Applications/upx -d /Applications/xf-adsk20.app/Contents/MacOS/x-force，然后按回车键，提示输入密码，这里输入密码不显示，输完密码直接按回车键，返回提示“Unpacked 1 file.”即操作成功。