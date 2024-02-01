
今天在[[Archlinux]] [Wiki](https://wiki.archlinux.org/) 中看到：
- **UKUI** — [UKUI](https://www.ukui.org/) is a lightweight Linux desktop environment, developed based on GTK and Qt. UKUI is the default desktop environment for Ubuntu kylin.
- [项目介绍](https://gitee.com/openkylin/docs/blob/master/社区产品/UKUI/UKUI介绍.md)：UKUI(Ultimate Kylin User Interface) SIG小组致力于桌面环境相关软件包的规划、维护和升级工作，满足各种设备和用户需求的桌面环境程序，主要包含程序启动器（开始菜单）、用户配置、文件管理、登录锁屏、桌面、网络工具、快捷配置等，为用户提供基本的图形化操作平台。桌面核心组件开发工具以Qt、C++为主，宗旨是始终如一地提升系统的操作体验，提供集稳定性、美观性、流畅性和便捷性为一体的桌面环境。

不知道这个桌面环境在轻量方面到底如何？我的那台老旧笔记本 DELL vostro 1450 能否拖得起？前天刚尝试安装了一个 [[20240123-关于 Linux 类操作系统的选择问题|Debian]] 系统，但选择安装的桌面环境 **[KDE Plasma](https://wiki.archlinux.org/title/KDE_Plasma "KDE Plasma")** 慢得无法忍受。周一晚上回去更换这个 UKUI 试试。

如果还不行，我打算可能继续使用以前曾用过的 [[Archlinux]] 了。

**下面是安装方法：**

1. 在安装UKUI之前，我们首先需要更新系统。打开终端，使用以下命令更新软件包列表和已安装的软件包：

	sudo apt update
	sudo apt upgrade

2. 添加UKUI源

要安装UKUI，我们需要添加UKUI源。在终端中运行以下命令以添加UKUI源：

	sudo sh -c 'echo "deb $(lsb_release -cs) main" >> /etc/apt/sources.list.d/ukui.list'

3. 导入UKUI密钥

为了确保软件包的安全性，我们需要导入UKUI的密钥。在终端中运行以下命令以导入密钥：

	wget
	sudo apt-key add keyring.gpg

4. 更新系统并安装UKUI

更新系统以使新添加的源生效，并使用以下命令安装UKUI桌面环境：

	sudo apt install ukui-desktop-environment

5. 选择UKUI作为默认桌面环境

安装完成后，我们需要选择UKUI作为默认的桌面环境。在登录界面，点击右上角的设置图标，选择UKUI作为默认的桌面环境，然后输入密码登录即可。

**如果你只想安装UKUI桌面而不是整个UKUI桌面环境，可以按照以下步骤进行操作：**

1. 我们需要更新系统。见上面第一步
2. 安装UKUI桌面，在终端中运行以下命令以安装UKUI桌面：
	sudo apt install ukui-desktop
3. 选择UKUI作为默认桌面环境