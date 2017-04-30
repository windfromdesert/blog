## Archlinux 安装

Archlinux 能在任何内存空间不小于256MB的I686兼容机上运行，最基本的base组中包含的包将占用约800MB的存储空间。也可以手动挑选base组中的包进行安装。

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
            +   使用 netctl 的 wifi-menu 连接到无线网络
                -   如果有多个无线设备，使用 `# iw dev` 可以确定无线网络接口名。[一般以"w"开头]
                -   然后使用 netctl 的 wifi-menu 来连接网络：例如：`# wifi-menu wlp3s0`
            +   不用 wifi-menu
                -   激活无线网络接口，例如：`# link set wlp3s0 up`，可以使用 `# ip link show wlp3s0` 来检验接口是否激活成功。
                -   无线网卡的固件加载：可以使用 dmesg 来查询内核日志，`# dmesg | grep firmware`
                -   扫描网络，例如：`# iw dev wlp3s0 scan | grep SSID` ，这样就可以得到 SSID
                -   连接特定网络，例如：`# wpa_supplicant -B -i wlp3s0 -c <(wpa_passphrase "SSID" "psk")`。[将 SSID 替换为实际的网络名称，psk 替换为无线密码，请保留引号]
                -   最后，用 dhcpcd 获得 IP 分配。例如：`# dhcpcd wlp3s0`
    +   准备存储设备
