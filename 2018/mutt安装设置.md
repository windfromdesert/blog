### mutt的安装与设置--Archlinux

mutt是linux下的一个email程序。Mutt 显然是一个 Unix 的邮件程序，它跟一般的 Windows 邮件程序不同，它不是一个包罗万象的大杂烩。你甚至会发现它根本不直接发出邮件，它从来不自己编辑邮件，它从来不自己对邮件进行加密和数字签名……Mutt 更像一个文件管理器，只不过它管理的是email。它的功能是借助各个最强大的程序来实现的。这符合 UNIX 的设计思想。----选自《百度百科》

#### 安装 mutt 和 offlineimap

`# pacman -S mutt offlineimap`

收邮件我选择使用第三方 offlineimap，因为 mutt 自带的 imap 模块我总是无法实现将邮件下载到本地；发邮件我选择使用 mutt 自带模块来处理。

mutt 的配置文件在 `~/.mutt/` 目录，有三个文件： muttrc 是主配置文件，mutt.alias 是邮箱地址簿文件，mailcap 是设置哪些附件可以通过插件或直接浏览的。我的配置文件模板在 [这里][1] 有个示例。


[1]: https://github.com/windfromdesert/code/tree/master/mutt "我的 mutt 配置文件"
