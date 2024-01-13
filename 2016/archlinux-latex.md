## [[Archlinux]] 中安装 Latex

使用最小化安装：

    # pacman -S texlive-core texlive-bin texlive-latexextra

安装后为了转换成 PDF 时能支持中文，将使用 xelatex 作为转换引擎。

编写 [zhfongcfg.sty][1] 中文模板文件。然后将模板文件放到以下目录：`# /usr/share/texmf-dist/tex/latex/base/`。然后在该目录输入以下命令：

    # mktexlsr //这是为了刷新一下tex目录结构

这样中文模板就可以使用了。使用方法为在 tex 文档中加入语句：`\usepackage{zhfontcfg}`

[1]: https://github.com/windfromdesert/code/blob/master/zhfontcfg.sty "xelatex 中文模板"
