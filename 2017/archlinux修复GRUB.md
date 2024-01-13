### Archlinux 修复 GRUB

1. 使用 [[Archlinux]] 安装启动盘进入安装界面。
2. 使用如下命令确定 Archlinux 安装在哪个盘。

        #lsblk
    
3. 然后向终端中逐条输入下列命令：（＃是指管理员权限）

        #mount -t jfs /dev/sda7 /mnt
        #mount -t ext3 /dev/sda8 /mnt/boot //如果没有划分单独的 boot 盘，这句就不需要
        #mount -t proc /proc /mnt/proc
        #mount -t sysfs sys /mnt/sys
        #mount -o bind /dev /mnt/dev

说明一下，第一句就是将原来系统的根目录所在分区挂载到livecd的/mnt中，第二句是将原来系统中/boot目录所在分区挂载到livecd的/mnt/boot，这两句的先后顺序不能换。第三句往后直接照抄就行，这几句都是挂载目录，为一会儿的在livecd中使用原来系统做准备。以上命令输完之后输入下一条命令：

    #chroot /mnt /bin/bash

输入这条命令之后你之后输入的所有内容都是在原来系统中操作了。现在可以进行正式的grub修复了。

然后可以参考安装 Archlinux 的步骤重新安装一遍 GRUB，从而修复系统引导。
