## Archlinux 下安装 Pandoc 与 PDF 转换插件

在 [[Archlinux]] 下安装 Pandoc 很简单，直接 `# pacman -S pandoc` 即可。

但默认状态下，Pandoc 并不能完成 PDF 的转换。要实现 PDF 的转换还需要安装 texlive-core，可以如下进行安装：

    # pacman -S texlive-core texlive-bin texlive-latexextra

这样安装完 texlive-core 后，就可以使用以下命令将 markdown 文件转换为 PDF了。

    $ pandoc sample.md -o sample.pdf

但这样转换后会发现文档并不支持中文。为了转换时能正确显示中文，我们应该使用 xelatex 引擎，而且还要设置一下 xelatex 转换 PDF 时的中文模板。

默认模板文件：`$ /usr/share/pandoc/data/templates/default.latex`

最好的办法是自己设置一个模板文件，比如我的是：[cn.tex][1]，需注意的是文档中的中文字体需换成自己系统中安装的字体。系统中安装的字体名称可以使用命令：`$ fc-list` 来查询。

好了，这样就可以使用以下命令来转换中文 markdown 文件为 PDF 文件了。

    $ pandoc sample.md -o sample.pdf --latex-engine=xelatex --template=cn.tex

[1]: https://github.com/windfromdesert/code/blob/master/cn.tex "pandoc latex配置文件"
