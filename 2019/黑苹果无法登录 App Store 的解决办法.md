### [[黑苹果]] 无法登录 App Store 的解决办法

步骤：

1. 先安装有线网卡驱动；
2. 再安装无线网卡驱动；
3. 再连接蓝牙设备；

这样做的目的是：让有线网卡的BSD名称为en0,让无线网卡的BSD名称为en1,让蓝牙设备的BSD名称为en2

如果BSD名称已经乱掉，可以用以下方法：

打开Find—\>系统—\>资源库—\>Preferences—\>SystemConfiguration文件夹—\>删除文件NetworkInterfaces.plist

然后打开系统偏好设置，将所有网络接口全部删除，重启电脑后，按上述步骤: 安装有线网卡驱动、无线网卡驱动、蓝牙驱动。