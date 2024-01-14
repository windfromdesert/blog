## Rsync 详解

1、什么是Rsync

Rsync（remote synchronize）是一个远程数据同步工具，可通过LAN/WAN快速同步多台主机间的文件。Rsync使用所谓的“Rsync算法”来使本地和远程两个主机之间的文件达到同步，这个算法只传送两个文件的不同部分，而不是每次都整份传送，因此速度相当快。

Rsync本来是用于替代rcp的一个工具，目前由rsync.samba.org维护，所以rsync.conf文件的格式类似于samba的主配置文件。Rsync可以通过rsh或ssh使用，也能以daemon模式去运行，在以daemon方式运行时Rsync server会打开一个873端口，等待客户端去连接。连接时，Rsync server会检查口令是否相符，若通过口令查核，则可以开始进行文件传输。第一次连通完成时，会把整份文件传输一次，以后则就只需进行增量备份。

Rsync支持大多数的类Unix系统，无论是Linux、Solaris还是BSD上都经过了良好的测试。此外，它在windows平台下也有相应的版本，如cwRsync和Sync2NAS等工具。

Rsync的基本特点如下：

1.  可以镜像保存整个目录树和文件系统；
2.  可以很容易做到保持原来文件的权限、时间、软硬链接等；
3.  无须特殊权限即可安装；
4.  优化的流程，文件传输效率高；
5.  可以使用rsh、ssh等方式来传输文件，当然也可以通过直接的socket连接；
6.  支持匿名传输。

2、Rsync同步算法

Rsync之所以同步文件的速度相当快，是因为“Rsync同步算法”能在很短的时间内计算出需要备份的数据，关于Rsync的同步算法描述如下：

假定在1号和2号两台计算机之间同步相似的文件A与B，其中1号对文件A拥有访问权，2号对文件B拥有访问权。并且假定主机1号与2号之间的网络带宽很小。那么rsync算法将通过下面的五个步骤来完成：

1、2号将文件B分割成一组不重叠的固定大小为S字节的数据块，最后一块可能会比S 小。

2、2号对每一个分割好的数据块执行两种校验：一种是32位的滚动弱校验，另一种是128位的MD4强校验

3、2号将这些校验结果发给1号。

4、1号通过搜索文件A的所有大小为S的数据块(偏移量可以任选，不一定非要是S的倍数)，来寻找与文件B的某一块有着相同的弱校验码和强校验码的数据块。这项工作可以借助滚动校验的特性很快完成。

5、1号发给2号一串指令来生成文件A在2号上的备份。这里的每一条指令要么是对文件B经拥有某一个数据块而不须重传的证明，要么是一个数据块，这个数据块肯定是没有与文件B的任何一个数据块匹配上的。

3、Rsync参数说明

3.1 rsyncd.conf配置文件

－、全局参数
在文件中[module]之前的所有参数都是全局参数，当然也可以在全局参数部分定义模块参数，这时候该参数的值就是所有模块的默认值。

port

指定后台程序使用的端口号，默认为873。

motd file

"motd file"参数用来指定一个消息文件，当客户连接服务器时该文件的内容显示给客户，默认是没有motd文件的。

log file

"log file"指定rsync的日志文件，而不将日志发送给syslog。比如可指定为“/var/log/rsyncd.log”。

pid file

指定rsync的pid文件，通常指定为“/var/run/rsyncd.pid”。

syslog facility

指定rsync发送日志消息给syslog时的消息级别，常见的消息级别是：uth, authpriv, cron, daemon, ftp, kern, lpr, mail, news, security, sys-log, user, uucp, local0, local1, local2, local3,local4, local5, local6和local7。默认值是daemon。

二、模块参数

主要是定义服务器哪个目录要被同步。其格式必须为“[module]”形式，这个名字就是在rsync 客户端看到的名字，其实有点象Samba服务器提供的共享名。而服务器真正同步的数据是通过 path 来指定的。我们可以根据自己的需要，来指定多个模块，模块中可以定义以下参数：

comment

给模块指定一个描述，该描述连同模块名在客户连接得到模块列表时显示给客户。默认没有描述定义。

path

指定该模块的供备份的目录树路径，该参数是必须指定的。

use chroot

如 果"use chroot"指定为true，那么rsync在传输文件以前首先chroot到path参数所指定的目录下。这样做的原因是实现额外的安全防护，但是缺点是需要以roots权限，并且不能备份指向外部的符号连接所指向的目录文件。默认情况下chroot值为true。

uid

该选项指定当该模块传输文件时守护进程应该具有的uid，配合gid选项使用可以确定哪些可以访问怎么样的文件权限，默认值是"nobody"。

gid

该选项指定当该模块传输文件时守护进程应该具有的gid。默认值为"nobody"。

max connections

指定该模块的最大并发连接数量以保护服务器，超过限制的连接请求将被告知随后再试。默认值是0，也就是没有限制。

list

该选项设定当客户请求可以使用的模块列表时，该模块是否应该被列出。如果设置该选项为false，可以创建隐藏的模块。默认值是true。

read only

该选项设定是否允许客户上载文件。如果为true那么任何上载请求都会失败，如果为false并且服务器目录读写权限允许那么上载是允许的。默认值为true。

exclude

用 来指定多个由空格隔开的多个文件或目录(相对路径)，并将其添加到exclude列表中。这等同于在客户端命令中使用--exclude来指定模式，一个 模块只能指定一个exclude选项。但是需要注意的一点是该选项有一定的安全性问题，客户很有可能绕过exclude列表，如果希望确保特定的文件不能 被访问，那就最好结合uid/gid选项一起使用。

exclude from

指定一个包含exclude模式的定义的文件名，服务器从该文件中读取exclude列表定义。

include

用来指定不排除符合要求的文件或目录。这等同于在客户端命令中使用--include来指定模式，结合include和exclude可以定义复杂的exclude/include规则。

include from

指定一个包含include模式的定义的文件名，服务器从该文件中读取include列表定义。

auth users

该 选项指定由空格或逗号分隔的用户名列表，只有这些用户才允许连接该模块。这里的用户和系统用户没有任何关系。如果"auth users"被设置，那么客户端发出对该模块的连接请求以后会被rsync请求challenged进行验证身份这里使用的 challenge/response认证协议。用户的名和密码以明文方式存放在"secrets file"选项指定的文件中。默认情况下无需密码就可以连接模块(也就是匿名方式)。

secrets file

该 选项指定一个包含定义用户名:密码对的文件。只有在"auth users"被定义时，该文件才有作用。文件每行包含一个username:passwd对。一般来说密码最好不要超过8个字符。没有默认的 secures file名，需要限式指定一个(例如：/etc/rsyncd.passwd)。注意：该文件的权限一定要是600，否则客户端将不能连接服务器。

strict modes

该选项指定是否监测密码文件的权限，如果该选项值为true那么密码文件只能被rsync服务器运行身份的用户访问，其他任何用户不可以访问该文件。默认值为true。

hosts allow

该选项指定哪些IP的客户允许连接该模块。客户模式定义可以是以下形式：

单个IP地址，例如：192.167.0.1

整个网段，例如：192.168.0.0/24，也可以是192.168.0.0/255.255.255.0

多个IP或网段需要用空格隔开，“\*”则表示所有，默认是允许所有主机连接。

hosts deny

指定不允许连接rsync服务器的机器，可以使用hosts allow的定义方式来进行定义。默认是没有hosts deny定义。

ignore errors

指定rsyncd在判断是否运行传输时的删除操作时忽略server上的IO错误，一般来说rsync在出现IO错误时将将跳过--delete操作，以防止因为暂时的资源不足或其它IO错误导致的严重问题。

ignore nonreadable

指定rysnc服务器完全忽略那些用户没有访问权限的文件。这对于在需要备份的目录中有些文件是不应该被备份者得到的情况是有意义的。

lock file

指定支持max connections参数的锁文件，默认值是/var/run/rsyncd.lock。

transfer logging

使rsync服务器使用ftp格式的文件来记录下载和上载操作在自己单独的日志中。

log format

通过该选项用户在使用transfer logging可以自己定制日志文件的字段。其格式是一个包含格式定义符的字符串，可以使用的格式定义符如下所示：

%h 远程主机名

%a 远程IP地址

%l 文件长度字符数

%p 该次rsync会话的进程id

%o 操作类型："send"或"recv"

%f 文件名

%P 模块路径

%m 模块名

%t 当前时间

%u 认证的用户名(匿名时是null)

%b 实际传输的字节数

%c 当发送文件时，该字段记录该文件的校验码

默认log格式为："%o %h [%a] %m (%u) %f %l"，一般来说,在每行的头上会添加"%t [%p] "。在源代码中同时发布有一个叫rsyncstats的perl脚本程序来统计这种格式的日志文件。

timeout

通过该选项可以覆盖客户指定的IP超时时间。通过该选项可以确保rsync服务器不会永远等待一个崩溃的客户端。超时单位为秒钟，0表示没有超时定义，这也是默认值。对于匿名rsync服务器来说，一个理想的数字是600。

refuse options

通过该选项可以定义一些不允许客户对该模块使用的命令参数列表。这里必须使用命令全名，而不能是简称。但发生拒绝某个命令的情况时服务器将报告错误信息然后退出。如果要防止使用压缩，应该是："dont compress = *"。

dont compress

用来指定那些不进行压缩处理再传输的文件，默认值是*.gz *.tgz *.zip *.z *.rpm *.deb *.iso *.bz2 *.tbz

3.2 Rsync命令
在对rsync服务器配置结束以后，下一步就需要在客户端发出rsync命令来实现将服务器端的文件备份到客户端来。rsync是一个功能非常强大的工具，其命令也有很多功能特色选项，我们下面就对它的选项一一进行分析说明。

Rsync的命令格式可以为以下六种：

　　rsync [OPTION]... SRC DEST

　　rsync [OPTION]... SRC [USER@]HOST:DEST

　　rsync [OPTION]... [USER@]HOST:SRC DEST

　　rsync [OPTION]... [USER@]HOST::SRC DEST

　　rsync [OPTION]... SRC [USER@]HOST::DEST

　　rsync [OPTION]... rsync://[USER@]HOST[:PORT]/SRC [DEST]

对应于以上六种命令格式，rsync有六种不同的工作模式：

1)拷贝本地文件。当SRC和DES路径信息都不包含有单个冒号":"分隔符时就启动这种工作模式。如：rsync -a /data /backup

2)使用一个远程shell程序(如rsh、ssh)来实现将本地机器的内容拷贝到远程机器。当DST路径地址包含单个冒号":"分隔符时启动该模式。如：rsync -avz *.c foo:src

3)使用一个远程shell程序(如rsh、ssh)来实现将远程机器的内容拷贝到本地机器。当SRC地址路径包含单个冒号":"分隔符时启动该模式。如：rsync -avz foo:src/bar /data

4)从远程rsync服务器中拷贝文件到本地机。当SRC路径信息包含"::"分隔符时启动该模式。如：rsync -av root@172.16.78.192::www /databack

5)从本地机器拷贝文件到远程rsync服务器中。当DST路径信息包含"::"分隔符时启动该模式。如：rsync -av /databack root@172.16.78.192::www

6)列远程机的文件列表。这类似于rsync传输，不过只要在命令中省略掉本地机信息即可。如：rsync -v rsync://172.16.78.192/www

rsync参数的具体解释如下：

    -v, --verbose 详细模式输出
    -q, --quiet 精简输出模式
    -c, --checksum 打开校验开关，强制对文件传输进行校验
    -a, --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD
    -r, --recursive 对子目录以递归模式处理
    -R, --relative 使用相对路径信息
    -b, --backup 创建备份，也就是对于目的已经存在有同样的文件名时，将老的文件重新命名为~filename。可以使用--suffix选项来指定不同的备份文件前缀。
    --backup-dir 将备份文件(如~filename)存放在在目录下。
    -suffix=SUFFIX 定义备份文件前缀
    -u, --update 仅仅进行更新，也就是跳过所有已经存在于DST，并且文件时间晚于要备份的文件。(不覆盖更新的文件)
    -l, --links 保留软链结
    -L, --copy-links 想对待常规文件一样处理软链结
    --copy-unsafe-links 仅仅拷贝指向SRC路径目录树以外的链结
    --safe-links 忽略指向SRC路径目录树以外的链结
    -H, --hard-links 保留硬链结
    -p, --perms 保持文件权限
    -o, --owner 保持文件属主信息
    -g, --group 保持文件属组信息
    -D, --devices 保持设备文件信息
    -t, --times 保持文件时间信息
    -S, --sparse 对稀疏文件进行特殊处理以节省DST的空间
    -n, --dry-run现实哪些文件将被传输
    -W, --whole-file 拷贝文件，不进行增量检测
    -x, --one-file-system 不要跨越文件系统边界
    -B, --block-size=SIZE 检验算法使用的块尺寸，默认是700字节
    -e, --rsh=COMMAND 指定使用rsh、ssh方式进行数据同步
    --rsync-path=PATH 指定远程服务器上的rsync命令所在路径信息
    -C, --cvs-exclude 使用和CVS一样的方法自动忽略文件，用来排除那些不希望传输的文件
    --existing 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件
    --delete 删除那些DST中SRC没有的文件
    --delete-excluded 同样删除接收端那些被该选项指定排除的文件
    --delete-after 传输结束以后再删除
    --ignore-errors 及时出现IO错误也进行删除
    --max-delete=NUM 最多删除NUM个文件
    --partial 保留那些因故没有完全传输的文件，以是加快随后的再次传输
    --force 强制删除目录，即使不为空
    --numeric-ids 不将数字的用户和组ID匹配为用户名和组名
    --timeout=TIME IP超时时间，单位为秒
    -I, --ignore-times 不跳过那些有同样的时间和长度的文件
    --size-only 当决定是否要备份文件时，仅仅察看文件大小而不考虑文件时间
    --modify-window=NUM 决定文件是否时间相同时使用的时间戳窗口，默认为0
    -T --temp-dir=DIR 在DIR中创建临时文件
    --compare-dest=DIR 同样比较DIR中的文件来决定是否需要备份
    -P 等同于 --partial
    --progress 显示备份过程
    -z, --compress 对备份的文件在传输时进行压缩处理
    --exclude=PATTERN 指定排除不需要传输的文件模式
    --include=PATTERN 指定不排除而需要传输的文件模式
    --exclude-from=FILE 排除FILE中指定模式的文件
    --include-from=FILE 不排除FILE指定模式匹配的文件
    --version 打印版本信息
    --address 绑定到特定的地址
    --config=FILE 指定其他的配置文件，不使用默认的rsyncd.conf文件
    --port=PORT 指定其他的rsync服务端口
    --blocking-io 对远程shell使用阻塞IO
    -stats 给出某些文件的传输状态
    --progress 在传输时现实传输过程
    --log-format=formAT 指定日志文件格式
    --password-file=FILE 从FILE中得到密码
    --bwlimit=KBPS 限制I/O带宽，KBytes per second
    -h, --help 显示帮助信息

常用增量备份命令：

	rsync -ahuvz SRC DEST
	rsync -ahuv SRC DEST / 其中 z 表示传输时进行压缩处理
