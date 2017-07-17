### 关于选择字体

+	总原则是：CRT、LCD 等低 ppi 屏幕上阅读大量文字请使用无衬线字体；在印刷品上阅读大量文字请使用衬线字体。
+	Windows Vista 之前的 Windows 一直用的中易宋体（衬线字体），实在是不该。好在 Vista 之后改成微软雅黑（无衬线字体）了。长时间在 CRT、LCD 上阅读衬线字体非常容易疲劳，不信你找篇长文章用五号字的“华文楷体”阅读试试……
+	印刷品上衬线字体比无衬线字体看着不易疲劳，所以大部分报纸上正文都是小五号报宋体（衬线字体），只有标题为了题目才会用无衬线字体。
+	Kindle 由于采用了 167 ppi 的高 ppi 的 E-Ink 屏幕，比电脑屏幕的 72 ppi 90-120 dpi 高出一倍多了（引用来源），比报纸的 100-150 dpi 也高出一些，所以 *完全可以当作印刷品对待* ，所以应该使用衬线字体。

所以这样选字体：

+	在低 ppi 的电脑屏幕上，推荐使用无衬线中文字体，比如微软的商业字体“微软雅黑”，自由开源的“文泉驿徽米黑”、“文泉驿正黑”也是不错的选择。
+	在高 ppi 的 Kindle 的 E-Ink 屏幕上，推荐使用衬线中文字体，比如方正雅宋系列里的“方正准雅宋”。如果你习惯在 Kindle 上看小字体的话，你甚至可以选择“方正报宋”“汉仪报宋”这样的适合小字号的笔画很细的衬线字体。（报纸上用的就是这样的字体）

### 优化字体

#### 更改 Locale

这个步骤是为了让目录界面不出现□□□□。方法就是在目录界面，按 Enter 键，然后输入下面三行 debug 代码，每行按 Enter 结束，然后再按 Enter 再打开新的输入框。

	;debugOn				#打开 debug 模式
	~changeLocale zh-CN.utf8		#更改 Local 为 zh-CN.utf8
	;debugOff				#关闭 debug 模式，可以不输入

这个命令是大小写不敏感的，我区分大小写只是为了好看……另外，在 debug 模式开启的时候输入 `~help` 可以查看有哪些 debug 命令哦。

执行完这三行命令之后，是需要重启才能应用的，不过先别急着重启，越狱的时候一起重启吧。

#### 越狱

这个不用说了吧，是装软件的前提。从上文那个 MobileRead [链接][1] 中下载 update-jailbreak-*.zip，并解压出对应你的机器的版本的update_jailbreak_*_install.bin 文件，放到 Kindle 的根目录中，然后在 Kindle 的 Settings 页面中，按 Menu，选择 Update your Kindle，等待两分钟左右，即可完成越狱，并且由于已经重启了，上一步的 Locale 也就生效了。这时 Kindle 的根目录中应该已经多出了linkjail 这个目录了。在 Kindle 的内部文件系统中，相应的目录已经被换成了符号链接，目标就是这个 linkjail，我们可以通过修改linkjail 中的文件来达到修改 Kindle 内部文件系统文件的目的。

#### 安装 Fonts Hack

依然是在那个链接中，下载 update-fonts-*.zip，解压出对应版本的 update_fonts_*_install.bin 文件，放到 Kindle 的根目录中，在 Kindle 的 Settings 页面中，按 Menu，选择 Update your Kindle，等待两分钟左右，即可完成安装。

#### 复制自定义字体

这时再连接 Kindle，便可发现根目录下多了 linkfonts 目录，同样，这也是 Kindle 内部文件系统中某些符号链接的目标，在linkfonts/fonts 目录下，可以看到一堆字体。请把你准备好的四个中文字体文件（普通、粗体、斜体、粗斜体）复制到linkfonts/fonts 目录下，保持字体原来的文件名也是可以的，不过为了方便下文引用，最好把它们命名成下面这样，另外，不要用中文名……

	CJK_Regular.ttf → 普通字体，用于正文的显示
	CJK_Bold.ttf → 粗体字体，用于粗体的显示
	CJK_Italic.ttf → 斜体字体，用于斜体的显示
	CJK_BoldItalic.ttf →粗斜字体，用于粗斜体的显示

注意：这个目录中还有一个 CJK.ttf，它是 Droid Sans Fallback，即 Android 中默认的中文字体（无衬线！），是 Fonts hack 用来代替原生的中文字体的，理论上删除它，然后引用别的字体也是没有关系的，但是由于 Fonts hack 的一些保险装置，删除它之后会导致 Fonts hack 自动失效，所以还是留着它吧，不引用它就好了。

#### 更改配置文件

上一步已经把所需要用到的四个字体复制到 linkfonts/fonts 目录了，但是它们是不会被自动使用的，需要更改配置文件让 Kindle 去使用它们。

打开 linkfonts/etc 目录，可以看到一堆字体的配置文件。Linux 桌面版用户对这些文件应该很熟悉了……我们要动刀的是fonts.properties, fonts.properties-3, fonts,properties-3-nocondensed 这三个文件。

注意：这三个文件都是 Unix/Linux/OS X 的 LF 换行格式，千万别用 Windows 自带的“记事本”去编辑保存它！否则会变成 CR+LF 的换行格式！Windows 用户请使用 Notepad++ 去编辑，或者下载我编辑好的文件。

这三个文件的内容大同小异，是在不同的阅读模式下的字体渲染设置。主要是改 # international font support 这一行后面的一些内容。默认是这个样子的：

	# international font support
	code2000.0=I18N.ttf
	code2000.plain=I18N.ttf

	jpan.0=CJK.ttf
	jpan.plain=CJK.ttf
	jpan.1=CJK.ttf
	jpan.bold=CJK.ttf

	kore.0=CJK.ttf
	kore.plain=CJK.ttf
	kore.1=CJK.ttf
	kore.bold=CJK.ttf

	hant.0=CJK.ttf
	hant.plain=CJK.ttf
	hant.1=CJK.ttf
	hant.bold=CJK.ttf

	hans.0=CJK.ttf
	hans.plain=CJK.ttf
	hans.1=CJK.ttf
	hans.bold=CJK.ttf

可以看出，最上面一段是对符号字体（这是最后一个 Fallback，关于 Fallback 的顺序，请参见 [这个帖子][2] ）进行设置，不用修改，后面四段内容分别对日文、谚文、正体中文和简体中文进行了设置，格式如下：

[字符集].[样式]=[字体文件名]

“字符集”无非 CJK，而样式则是有八种的，比如系统原生 Serif 的设置：

	# Serif font definition
	#
	serif.0=Serif_Regular.ttf
	serif.1=Serif_Bold.ttf
	serif.2=Serif_Italic.ttf
	serif.3=Serif_BoldItalic.ttf
	serif.plain=Serif_Regular.ttf
	serif.bold=Serif_Bold.ttf
	serif.italic=Serif_Italic.ttf
	serif.bolditalic=Serif_BoldItalic.ttf

可以看出有 0, 1, 2, 3, plain, bold, italic, bolditalic 八种，并且应该是两两对应的。明白了这一点，就很容易了，把刚才那四段定义 CJK 字体的内容改成这模样就行了：

	# international font support
	code2000.0=I18N.ttf
	code2000.plain=I18N.ttf

	jpan.0=CJK_Regular.ttf
	jpan.1=CJK_Bold.ttf
	jpan.2=CJK_Italic.ttf
	jpan.3=CJK_BoldItalic.ttf
	jpan.plain=CJK_Regular.ttf
	jpan.bold=CJK_Bold.ttf
	jpan.italic=CJK_Italic.ttf
	jpan.bolditalic=CJK_BoldItalic.ttf

	kore.0=CJK_Regular.ttf
	kore.1=CJK_Bold.ttf
	kore.2=CJK_Italic.ttf
	kore.3=CJK_BoldItalic.ttf
	kore.plain=CJK_Regular.ttf
	kore.bold=CJK_Bold.ttf
	kore.italic=CJK_Italic.ttf
	kore.bolditalic=CJK_BoldItalic.ttf

	hant.0=CJK_Regular.ttf
	hant.1=CJK_Bold.ttf
	hant.2=CJK_Italic.ttf
	hant.3=CJK_BoldItalic.ttf
	hant.plain=CJK_Regular.ttf
	hant.bold=CJK_Bold.ttf
	hant.italic=CJK_Italic.ttf
	hant.bolditalic=CJK_BoldItalic.ttf

	hans.0=CJK_Regular.ttf
	hans.1=CJK_Bold.ttf
	hans.2=CJK_Italic.ttf
	hans.3=CJK_BoldItalic.ttf
	hans.plain=CJK_Regular.ttf
	hans.bold=CJK_Bold.ttf
	hans.italic=CJK_Italic.ttf
	hans.bolditalic=CJK_BoldItalic.ttf

注意：这后面的字体名是可以改的，由于我在上一步的时候已经把自定义的字体名改成这模样了，所以是这样填的，如果你用的是其他的文件名的话，请对应修改。

正如上文所说，有 fonts.properties, fonts.properties-3, fonts,properties-3-nocondensed 这样三个文件，都要做类似的修改。修改完了之后用 Unix 的 LF 换行格式保存。

#### 重启 Kindle 的 Framework

这时候算是基本大功告成了，重启 Kindle 即可应用新字体。不过 Kindle 的完全重启（Settings - Restart your Kindle）时间有点长，所以 Fonts hack 中提供了只重启 Framework 的方法，即在拔掉 Kindle 之前，在 linkfonts 目录下面新建一个名为 reboot 的空文件。如果 Windows 用户不方便新建无扩展名的空文件的话，可以原地复制粘贴一下 autorebooot 这个文件然后再重命名为 reboot，它就是一个空文件……

这个文件建立之后，再拔掉 Kindle，就会看到屏幕左下角有一行字 Restarting framework... 过会儿就会重启完成，应用新字体了。这样比完全重启方便一些，也更快一些。

#### 换回拉丁字体

上文说了，由于我不仅要在 Kindle 上阅读中文书籍，还会阅读英语和德语的书籍，所以对拉丁字体的要求也比较高。而 Fonts hack 把原生的拉丁字体给换掉了！Serif（衬线）字体由 PMN Caecilia 换成了 Droid Serif，而 Sans Serif（无衬线）字体则由经典的 Helvetica 换成了 Droid Sans。后者的替换我没意见，反正我不用无衬线字体的，而前者的替换……令人发指。Droid Serif 实在是太丑了，跟中易宋体中的拉丁字符有得一拼……绝对不如原生的 PMN Caecilia 好看，所以我要换回去。有与我类似需要的同学可以继续读下去，不看英文书的可以跳过……

换回去的方法挺简单，在 linkfonts/backups 下面那些 Caecilia 开头的文件就是原生的 Serif 字体了。这里我被弄糊涂过，经过一些试验，我确认那八个字体中只有四个是原来的拉丁字体，另外四个带 Cond 字样的是紧密版的（即在 Aa 中选择 condensed 之后调用的字体）。对应关系如下：

Caecilia_LT_75_Bold.ttf → Serif_Bold.ttf
Caecilia_LT_76_Bold_Italic.ttf → Serif_BoldItalic.ttf
Caecilia_LT_66_Medium_Italic.ttf → Serif_Italic.ttf
Caecilia_LT_65_Medium.ttf → Serif_Regular.ttf

按照这样的对应关系把它们复制、替换掉 linkfonts/fonts 里的字体，然后再用上一节“重启 Kindle 的 Framework”中的方法，即可应用新字体。

[1]: http://www.mobileread.com/forums/showthread.php?t=88004 “MobileRead”
[2]: http://www.mobileread.com/forums/showpost.php?p=977006&postcount=97 “Fallback顺序”