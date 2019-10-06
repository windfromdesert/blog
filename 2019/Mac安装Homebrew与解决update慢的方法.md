### Mac安装Homebrew与解决update慢的方法

#### 安装 Homebrew

Homebrew 是什么？

使用 Homebrew 安装 Apple（或您的 Linux 系统）没有预装但 *你需要的东西*。

[Homebrew主页](https://brew.sh/index_zh-cn)

安装方法：

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

将以上命令粘贴至终端执行即可。

#### brew update，brew install慢如何解决？

brew使用国内镜像源  
这里用中科大的，另外还有清华的可用

```
# 步骤一
cd "$(brew --repo)"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
 
# 步骤二
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
 
#步骤三
brew update
```

注意这里需要等待一会，因为要更新资源。更新完后使用brew update，brew install速度变快很多了，不会卡在那半天没动静，替换镜像完成。

复原

```
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git
 
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core
 
brew update
```

brew镜像源

- 中科大brew镜像源 https://mirrors.ustc.edu.cn/brew.git
- 中科大brew镜像源 https://mirrors.ustc.edu.cn/homebrew-core.git
- 清华brew镜像源 https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
- 清华brew镜像源 https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
