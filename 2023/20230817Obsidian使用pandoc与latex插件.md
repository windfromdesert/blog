
1. 先去[pandoc官网](https://www.pandoc.org/installing.html)下载并安装pandoc；
2. 打开Obsidian，安装 pandoc 插件；
3. 安装 [basicLatex](https://www.tug.org/mactex/morepackages.html)，然后按 pandoc 网站说明使用命令更新字体(sudo tlmgr install collection-fontsrecommended)，如果提示权限不够，按提示给xxx文件夹授权：sudo chmod -R 777 xxx，再运行一下：sudo tlmgr update --self，sudo tlmgr install ctex
4. 进入 pandoc 插件选项页面，填写 export folder导出文件夹位置、pandoc命令位置（可以使用which pandoc查找）、pdflatex命令位置（可以使用which pdflatex查找）
5. 填写 Extra Pandoc arguments （额外参数）

	`--template`
	
	`/Users/wangbin/Documents/wb.latex`

> 上面的 wb.latex 是由自己创建的 latex 模板文件，文件名称一般是英文，可以随意取名。

- 查看pandoc默认LaTeX模板代码并保存成自己的名称
	`pandoc -D latex > /路径/wb.latex`

- 修改 wb.latex 内部代码，把 {\$documentclass\$} 修改为 {ctexart}
- 完工。