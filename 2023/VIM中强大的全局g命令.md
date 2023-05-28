
全局命令`:g`在Vim中有着意想不到强大的功能。当想要在整个文件中对于匹配的行或者非匹配行进行一些操作时，应该第一时间想到这个`:g`命令

```bash
:[range]global[!]/{pattern}/{command}
```

可以简写为

```bash
:[range]g/pattern/command
```

- \[range\]指定文本范围,默认为整个文档
- pattern在范围range内的行如果匹配pattern，则执行command
- !表示取反，也就是不匹配的行，也可以使用`vglobal`
- command默认是打印文本

整个命令可以理解成，在range范围内匹配pattern的行执行Ex command；可以通过`:help ex-cmd`来查看所有的Ex command

常用的Ex command：

- d 删除
- m 移动
- t 拷贝
- s 替换

## 示例

---

### 范围匹配

比如在20行到200行之间，每一行下插入空行

```bash
:20,200g/^/pu _
```

### 删除匹配的行

最简单的使用

```bash
:g/pattern/d
```

会删除与pattern匹配的行，再比如

```bash
:g/^$/d
```

可以用来删除空白行

### 删除不匹配的行

匹配使用`:g`，而不匹配有两种写法：

```bash
:g!/pattern/d
:v/pattern/d
```

`:v`是`:in(v)erse`的缩写，如果为了记忆的话，可以记住 inverse

### 删除大量匹配行

Vim在删除操作时，会先把要删除的内容放到寄存器中，假如没有指定寄存器，会默认放到一个未命名的寄存器中，对于要删除大量匹配行的行为，可能导致Vim花一些时间处理这些拷贝，避免花费不必要的时间可以指定一个blackhole寄存器`_`

```bash
:g/pattern/d_
```

### 移动匹配的行

将所有匹配的行移动到文件的末尾

```bash
:g/pattern/m$
```

### 复制匹配的行

将所有匹配的行复制到文件末尾

```bash
:g/pattern/t$
```

### 复制到 register a

Vim每个字母都是一个寄存器，所以使用全局命令也可以将内容复制到某一个寄存器，比如a

```bash
qaq:g/pattern/y A
```

- `qaq`清空寄存器a，`qa`开始记录命令到a寄存器，`q`停止记录
- `y A`将匹配的行A(append)追加到寄存器a中

存放到a寄存器之后就可以使用`"ap`来粘贴使用或者其他操作了

### 反转文件中的每一行

just show the power of :g

```bash
:g/^/m0
```

`:g`命令一行行匹配，匹配第一行时将第一行`m0`放到文件顶部，第二行放到文件顶部，当跑完一遍之后整个文件的每一行就反转了

### 在匹配行后添加文字

使用`s`命令可以实现，同样使用全局`g`命令也可以实现同样的效果

```bash
:g/pattern/s/$/mytext
```

这里使用到了`s`命令，substitute命令，可以使用`:help :s`来查看