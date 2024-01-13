### 使用 debtap 将 deb 安装包转换为 [[Archlinux]] 安装包

+ 安装 debtap （在AUR库中）

	    yaourt -S debtap

	    也应该安装bash， binutils ，pkgfile 和 fakeroot 依赖包。

+ 创建/更新 pkgfile 和 debtap 数据库。

	    sudo debtap -u

+ 转换deb包

	    debtap ***.deb

+ 安装

	    sudo pacman -U <package-name>
