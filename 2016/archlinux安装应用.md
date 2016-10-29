## Archlinux 我的安装应用

+   slock
    -   非常简单的屏幕锁定应用，直接 pacman -S slock 安装即可
    -   安装后只要在终端输入 slock 即可锁定屏幕
    -   锁定屏幕后碰触键盘或鼠标会发出声音并显示全色仍然锁定屏幕
    -   用键盘输入登录密码后，屏幕即解除锁定。

+   我的无线网卡 intel Wireless-AC 8260 安装过程中出现问题，无法连接网络，百试无解，最后在 [这里][1] 找到一段命令，很幸运地竟然解决了问题，特在此记录。命令如下：

        > sudo rmmod iwlmvm iwlwifi && sudo modprobe iwlmvm iwlwifi

[1]: https://forum.manjaro.org/t/problems-getting-network-address-via-wireless/5007/2 <Problems getting network address via wireless>
