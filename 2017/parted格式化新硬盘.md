## Parted格式化新硬盘详解

parted与fdisk作用一致，即对硬盘进行分区，但fdisk仅支持2T以下的硬盘，随着时代的发展，2T硬盘也逐渐普及，这时候就需要用到parted工具进行分区。

用法：

parted 硬盘：进入指定硬盘的操作模式
mklabel label-type(msdos、gpt)：设置指定类型分区，类型有"bsd", "dvh", "gpt",  "loop","mac", "msdos", "pc98", or "sun" 。
mkpart 分区类型(primary、extended、logical)：创建指定类型分区
Start：欲创建分区的开始柱面(默认单位是MB)
end：欲创建分区的结束柱面(默认单位是MB)
rm n：删除分区(n为分区号)
print：打印输出硬盘分区信息

1. 创建对齐分区：

将显示单位调整为Sector（大小512个字节）：

    (parted) unit s

列出当前逻辑卷：

    (parted) print

将原来Number1移除并且创建一个起始位为128 sector，小为976735934 sector的主分区。

    (parted) rm 1
    (parted) mkpart primary 128 976735934
    (parted) print

2. 对齐分区第二种方法：

1.获得你阵列的alignment参数（记得要将sdb替换为系统内核看到的设备名称）

    # cat /sys/block/sdb/queue/optimal_io_size
    1048576
    # cat /sys/block/sdb/queue/minimum_io_size
    262144
    # cat /sys/block/sdb/alignment_offset
    0
    # cat /sys/block/sdb/queue/physical_block_size
    512

2.把optimal_io_size的值与alignment_offset的值相加，之后除以physical_block_size的值。在我的例子中是：(1048576 + 0) / 512 = 2048。

3.这个数值是分区起始的扇区。新的parted命令应该写成类似下面这样

    mkpart primary 2048s 100%

2048s中的字母s是很有意义的：它告诉parted，你的输入是2048扇区，而不是2048字节，也不是2048兆字节。

4.如果一切顺利，分区将会被成功创建并没有任何警告信息。然后你就可以检查分区是否对齐了（如有必要，请将下面命令中的1替换为合适的分区号）。

    (parted) align-check optimal 1                                      
    1 aligned

正如我之前暗示的，会有一些特例，上面的做法对那些特例并不奏效：例如，如果optimal_io_size是0，可以分别尝试以下命令：

    (parted) mkpart primary 0% 100%
    (parted) mkpart primary 128s 100%

格式化磁盘为NTFS格式

    mkfs.ntfs -Q /dev/sdb1

注意上面命令中的 -Q 是指快速格式化，否则速度会非常非常慢。

ntfs 特性需要首先安装 ntfs-3g 

    # pacman -S ntfs-3g
