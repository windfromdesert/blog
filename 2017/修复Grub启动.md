## 修复 Grub 启动

Linux 在运行过程中，或是在一些情况下，会出现系统登录失败的问题，这时如果没有一套急救办法，那只有对着电脑屏幕干瞪眼了。

方法应该有很多，这里只介绍我碰到过的两种。

1.  开机启动失败并出现 grub rescue> 提示符

    这时可以先输入命令 `ls` 查看一下磁盘分区信息，一般会返回象这样的信息：(hd0,1),(hd0,2),(hd0,3),......

    然后可以输入命令 `set` 查看一下启动目录信息。然后依次测试boot目录在哪个磁盘。如果能测试出正确的磁盘信息，则使用如下命令:

    以下是/boot没有单独分区的命令：
        
        grub rescue>set root=(hd0,5)
        grub rescue>set prefix=(hd0,5)/boot/grub
        grub rescue>insmod /boot/grub/normal.mod

    以下是/boot 单独分区的命令：（这几句有待验证）

        grub rescue>set root=(hd0,5)
        grub rescue>set prefix=(hd0,5)/grub
        grub rescue>insmod /grub/normal.mod

    然后调用如下命令，就可以显示出丢失的grub菜单了。

        grub rescue>normal

    不过不要高兴，如果这时重启，问题依旧存在，我们需要进入Linux中，对grub进行修复。启动起来，进入系统之后，在终端执行代码:
    
        sudo update-grub
        sudo grub-install /dev/sda

    （sda是你的硬盘号码，千万不要指定分区号码，例如sda1，sda5等都不对）
    
    重启测试是否已经恢复了grub的启动菜单？ 恭喜你恢复成功！

2.  使用 livecd 进入装机界面，使用 chroot 命令修复 Grub

在开始前先说说用到的命令的简单说明。

首先是mount挂载命令: `mount -t type device mountpoint`，这条命令是将分区挂载到指定的目录，type是分区的格式，可以用的几个格式有：adfs,  affs,  autofs,  cifs,  coda,  coherent,  cramfs,debugfs, devpts, efs, ext, ext2, ext3, ext4, hfs, hfsplus, hpfs,iso9660, jfs, minix, msdos, ncpfs, nfs, nfs4, ntfs, proc,  qnx4,ramfs,reiserfs, romfs, smbfs, sysv, tmpfs, udf, ufs, umsdos,usbfs, vfat, xenix, xfs, xiafs，你需要根据自己分区选择对应的格式。

`mount -o -bind olddir newdir`，这条命令可以看出是转换作用目录的。

chroot用于改变程序执行时所参考的根目录位置。

终端下输入grub命令是进入grub提示符界面。

grub下输入root(hdX,Y)是指出Linux分区所在位置，一般X是0，Y则是你装的Linux根目录所在的分区号，注意这里是所在分区号有点小讲究，需要计算一下，比如我的是根目录是在/dev/sda7的，而硬盘上计数是从0开始的，所以这个第7个分区的分区号就应该是6，就是说Y就应该是6

启动LIVECD，用root登录，登录之后就可以开始工作了。首先回忆一下你的分区，哪个分区挂载的是哪个目录，这很重要。回忆一下/目录是在哪个分区是什么格式的，装系统的时候有没有为 /boot 目录另外分区，如果分了又是在哪个分区是什么格式。这里我的电脑上 / 目录是在sda7，格式是 ext4 ，装系统的时候我为 /boot 目录分了区，是sda8，分区格式是 ext3 。向终端中逐条输入下列命令：（＃是指管理员权限）

    #mount -t ext4 /dev/sda7 /mnt
    #mount -t ext3 /dev/sda8 /mnt/boot
    #mount -t proc /proc /mnt/proc
    #mount -t sysfs sys /mnt/sys
    #mount -o bind /dev /mnt/dev 

说明一下，第一句就是将原来系统的根目录所在分区挂载到livecd的/mnt中，第二句是将原来系统中/boot目录所在分区挂载到livecd的/mnt/boot，这两句的先后顺序不能换。第三句往后直接照抄就行，这几句都是挂载目录，为一会儿的在livecd中使用原来系统做准备。以上命令输完之后输入下一条命令：

    #chroot /mnt /bin/bash

重新安装引导器

GRUB
BIOS：

        # pacman -S grub os-prober
        # grub-install --target=i386-pc /dev/<目标磁盘>
        # grub-mkconfig -o /boot/grub/grub.cfg

UEFI：
        
        # pacman -S dosfstools grub efibootmgr
        # grub-install --target=x86_64-efi --efi-directory=<EFI 分区挂载点> --bootloader-id=grub
        # grub-mkconfig -o /boot/grub/grub.cfg

好了，到此就重新安装完了 grub 引导，退出重启使用命令： `exit`, `reboot` ，看看系统引导是否已经正确。
