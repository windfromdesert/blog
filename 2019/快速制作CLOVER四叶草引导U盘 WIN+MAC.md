### 快速制作CLOVER四叶草引导U盘 WIN+MAC

本教程将分别介绍从WINDOWS系统和MAC系统下快速创建CLOVER引导U盘最简单的制作方法

支持所有Legracy, EFI,主板，支持GPT,MDR分区格式， 支持windows, Mac ,ubuntu等系统引导。

本教程目录：

+ 一、WINDOWS系统下四叶草CLOVER引导U盘制作。
+ 二、MAC系统下四叶草CLOVER引导U盘制作。
+ 三、加入第三方驱动/插件。

在这篇教程里，你必需要用到的软件如下：

1、WINDOWS
+ 1）BOOT DISK Utility    链接: http://pan.baidu.com/s/1kTjItYv 密码: wqid
+ 2）Clover_v2.3k_r3277_USB.zip  链接同上

2、MAC
+ 1）系统自带磁盘工具
+ 2）EFI Tools Clover v2.3k r3259.zip   链接同上

3、准备安装OS X所需要加载的驱动

1）内核扩展
- FakeSMC.kext  2.5版本  （10.10）
- FakeSMC.kext  6.16版本（10.11）

2）鼠标键盘等驱动加载
- AppleACPIPS2Nub.kext   
- ApplePS2Controller.kext    
- VoodooPS2Controller.kext  

3）屏蔽原生电源管理
- NullCPUPowerManagement.kext 

除了软件，还需要一个8G以上的U盘。

正式进入主题

一、WINDOWS系统下四叶草CLOVER引导U盘制作。

1、进入WINDOWS系统，下载Boot Disk Utility ,解压并打开它。插入U盘；

2、点击Options设置，根据图选或者默认，点击OK；
制作不成功的请参照下面纠正：（需要点开U盘的＋下面的U盘）

3、选择你将要创建引导的U盘，点击farmat disk ，点击确定进行创建，并等待软件中间的信息过程完成结束；
（结束后，程序会自动添加四叶草引导相关文件，并把U盘名称命名为：CLOVER）

4、将下载好的 Clover\_v2.3k\_r3277\_USB.zip 解压，等到的文件全部复制到刚刚创建的CLover引导U盘。替换原文件。自此，WINDOWS系统下的CLOVR四叶草引导U盘制作完毕。

二、MAC系统下四叶草CLOVER引导U盘制作。（以我电脑现有的10.11为例）

1、下载EFI Tools Clover v2.3k r3259.zip和Clover\_v2.3k\_r3277\_USB.zip 两个压缩文件。并打开OS X系统下的  应用程序/实用工具/硬盘工具。

2、选择U盘，点击工具栏的抹掉，选择如图所示，名称：自取，格式：OS X 扩展（日志式），方案：GUID分区图，点击抹掉。

（提示，在此方式下抹掉的U盘，自带EFI分区，已经被隐藏。而显示出来或被加载的盘符可以用于OS X 系统安装盘的制作，或制作系统安装盘，请使用磁盘工具的恢复镜像方式，也可以使用Windows下安装Transmac来恢复。\*请勿格式化）

3、解压EFI Tools Clover v2.3k r3259.zip后得到EFI Tools Clover.app， 运行它后出现如上图界面，在终端界面中输入h,来安装CLOVER EFI, 以我的U盘为例，

/dev/disk3 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID\_partition\_scheme                        \*8.0 GB     disk3
   1:                        EFI EFI                     209.7 MB   disk3s1
   2:                  Apple\_HFS CLOVER                  7.7 GB     disk3s2

我的U盘EFI分区在BSD编号为：3，分区编号为：1。因此我输入3回车，1回车，输入y回车，再输入Y回车，此时输入苹果系统密码。等到安装完毕。其余按提示操作。

4、此时桌面上会加载一个EFI分区盘符。同样复制Clover\_v2.3k\_r3277\_USB.zip 解压后的文件到EFI分区覆盖原文件。至此。Mac系统下的CLOVR四叶草引导U盘制作完毕。

好了，我们重启电脑来看看制作好的CLOVER四叶草引导U盘吧。

三、加入第三方驱动/插件

为了更快的安装MAC OS 系统和更好的解决安装黑苹果系统所产生的总是，尽量使用第三方kexts来进行修复安装，本贴提供下载常见的必备kexts方便各果友的使用。

下载  鼠标键盘等驱动加载 AppleACPIPS2Nub.kext ApplePS2Controller.kext VoodooPS2Controller.kext  NullCPUPowerManagement.kext。复制到制作好的clover四叶草引导U盘，EFI/clover/kexts/10.10和EFI/clover/kexts/10.11下。没有自己新建一个。

FakeSMC.kext  2.5版本  （复制到10.10）
FakeSMC.kext  6.16版本（复制到10.11）
关于这两个。不知道我这样做是不是必要。但为了避免错误，还是需要将10.11对应最新的内核扩展kext比较好。
本教程结束。关于安装黑苹果和CLOVER设置和另外的内容请搜索论坛。如继续查看更多本人提供的教程，请关注。本人将陆续更新。
