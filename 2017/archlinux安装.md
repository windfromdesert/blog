## Archlinux 安装

[[Archlinux]] 能在任何内存空间不小于256MB的I686兼容机上运行，最基本的base组中包含的包将占用约800MB的存储空间。也可以手动挑选base组中的包进行安装。

+   下载 Archlinux 安装镜像ISO文件，并写入启动U盘。[ 这里我使用 USBWriter ]
+   重启电脑进入引导安装。
    -   如果使用 UEFI 主板（安装64位系统），且 UEFI 启动模式已启用，会自动通过 Gummiboot 启动 Archlinux，启动屏幕上会有相关提示。
    -   我安装的是32位系统。
+   建立网络连接
    -   安装程序会自动运行 dhcpcd 守护进程以尝试有线连接，可以使用 `$ ping -c 1 t.cn` 来测试是否连通。
    -   手动配置网络连接：有线连接
        +   确定有线连接接口名： ip link [有线连接一般以"e"开头]
        +   设置 /etc/dhcpcd.conf
        +   重启 dhcpcd.service：# systemctl restart dhcpcd.service
    -   手动配置网络连接：无线网络
        +   使用 netctl 的 wifi-menu 连接到无线网络。注：自从2020年6月之后 wifi-menu 就不可用了
            -   如果有多个无线设备，使用 `# iw dev` 可以确定无线网络接口名。[一般以"w"开头]
            -   然后使用 netctl 的 wifi-menu 来连接网络：例如：`# wifi-menu wlp3s0`
        +   使用 iwctl 连接到无线网络
            -   `[root@archiso~] iwctl` 进入[iwd#]
	    -   `[iwd#]device list` 查询机器的网卡设备。
	    -   `[iwd#]station <devicename> scan` 查询附近可用的wifi网络
	    -   `[iwd#]station <devicename> get-networks` 显示扫描的结果
	    -   `[iwd#]station <devicename> connect <wifi-ssid>` 连接wifi网络，如果wifi加密，会提示你输入密码
        +   不用 wifi-menu
            -   激活无线网络接口，例如：`# link set wlp3s0 up`，可以使用 `# ip link show wlp3s0` 来检验接口是否激活成功。
            -   无线网卡的固件加载：可以使用 dmesg 来查询内核日志，`# dmesg | grep firmware`
            -   扫描网络，例如：`# iw dev wlp3s0 scan | grep SSID` ，这样就可以得到 SSID
            -   连接特定网络，例如：`# wpa_supplicant -B -i wlp3s0 -c <(wpa_passphrase "SSID" "psk")`。[将 SSID 替换为实际的网络名称，psk 替换为无线密码，请保留引号]
            -   最后，用 dhcpcd 获得 IP 分配。例如：`# dhcpcd wlp3s0`
+   准备存储设备
    -   识别设备。`# lsblk`
    -   选择分区表类型：MBR-用于 BIOS 系统；GUID-用于 UEFI 系统。判断：`# parted /dev/sdx print`[其中 sdx 替换为需要查询的设备名，下同]
    -   分区。可以选择 parted fdisk cfdisk sfdisk gdisk cgdisk sgdisk，下面我使用 parted 进行分区。
        +   打开需要新建分区表的设备：`# parted /dev/sdx`
        +   为 BIOS 系统创建 MRB/MSDOS 分区表：`# (parted) mklabel msdos`
        +   为 UEFI 系统创建 GPT 分区表：`# (parted) mklabel gpt`
        +   分区：`# (parted) mkpart part-type fs-type start end`[其中 part-type 是分区类型，可以选择 primary,extended,logical,仅用于 MBR 分区表；fs-type 是文件系统类型，我选择的是 ext3,start 是分区的起始位置，可以带单位，例如 1M 指 1MiB;end 是设备的结束位置，不是与 start 值的差，同样可以带单位，也可以用百分比，例如 100% 表示到设备的末尾；为了不留空隙，分区的开始和结束应该首尾相连]
        +   设置 /boot 为启动目录：`# (parted) set partition boot on` [partition 是分区的编号，从 print 命令获取]
        +   创建文件系统。
            -   先查看所有分区：`# lsblk /dev/sdx`
            -   格式化分区：`# mkfs.ext4 /dev/sdxY`
            -   如果分了一个 swap 区，则需要格式化 swap 区：`mkswap /dev/sdaX`,`# swapon /dev/sdaX`
        +   挂载分区：注意这里不要挂载 swap。必须先挂载 /(root) 分区，其它目录都要在 / 分区中创建，然后再挂载。在安装环境中用 /mnt 目录挂载 root。`# mount /dev/sdax /mnt`, `# mkdir /mnt/home`,`# mount /dev/sday /mnt/home`,`# mkdir -p /mnt/boot`,`# mount /dev/sdxY /mnt/boot`
    -   选择安装镜像。`# nano /etc/pacman.d/mirrorlist`,安装镜像后请务必使用 `# pacman -Syy` 强制刷新
+   安装基本系统
    -   使用 pacstrap 命令：`# pacstrap -i /mnt base base-devel`[如果不想手动选择，可能忽略参数 -i，如果想通过 AUR 或者 ABS 编译安装软件包，则需要装上 base-devel]
+   生成 fstab
    -   `genfstab -U -p /mnt >> /mnt/etc/fstab`
    -   建议在生成 fstab 后检查一下是否正确。若在运行 genfstab 或是之后发生错误，请勿再次运行 genfstab，而是直接手动编辑 fstab 文件。
    -   如 noauto 位于 fstab 文件系统挂载参数之后，则不会在启动时挂载它。
    -   仅根分区需要在最后使用 1，其它可以使用 2 或 0 [btrfs 和 swap 分区的最后一列须设置成 0]
+   Chroot 并开始配置系统
    -   `arch-chroot /mnt /bin/bash`
    -   Locale:本地化程序与库。在下面两个文件设置，locale.gen 与 locale.conf
    -   /etc/locale.gen 是一个仅包含注释文档的文本文件，指定你需要的本地化类型，只需移除对应行前面的注释符号(#)即可。建议选择带 UTF-8 的项。
    -   `nano /etc/locale.gen`
    -   接着执行 locale-gen 以生成 locale 讯息：`# locale-gen`
    -   创建 locale.conf 并提交你的本地化选项：`# echo LANG=en_US.UTF-8 >> /etc/locale.conf`[注：这里一般不设置中文 locale，以避免 tty 乱码]
    -   时区：`# ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`
    -   硬件时间：`# hwclock --systohc --utc`
    -   Hostname: `# echo myhostname > /etc/hostname`，并在 /etc/hosts 添加同样的主机名。
    -   配置网络
        +   使用 `# ip link` 确认网络接口名。
        +   有线网络
        +   无线网络
	        + 使用 wifi-menu 。安装 dialog, wifi-menu依赖于它:  `# pacman -S dialog`
	        + 安装并重新启动电脑后，就可以使用 wifi-menu 来连接无线网络了。
    -   wpasswd 设置 Root 密码
    -   安装并配置 grub
        +   `pacman -S grub`
        +   `grub-install --target=i386-pc --recheck /dev/sda`[此处 sda 请自行调整，但后面千万不能添加数字]
        +   `pacman -S os-prober`[自动检测其它操作系统时需要安装它]
        +   `grub-mkconfig -o /boot/grub/grub.cfg`[自动生成配置文件]
    -   卸载分区并重启系统
        +   exit
        +   reboot[移除安装U盘，并还原 BIOS 启动选项，可以用 root 用户和设置的密码登录]
