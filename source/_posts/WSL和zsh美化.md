---
title: WSL和zsh美化
date: 2020-02-21 15:58:56
tags: 百宝箱
---
### 为啥要装WSL

Windows Subsystem for Linux（简称WSL）是一个在Windows 10上能够运行原生Linux二进制可执行文件（ELF格式）的兼容层

一方面是想继续熟悉linux，另一方面是windows实在是糟心，在没换mac之前只能拿wsl来过过瘾

yysy还是linux shell用得舒服

### 1.安装WSL

控制面板->程序和功能->启用或关闭Windows功能->勾选 适用于Linux的Windows子系统

[![img](https://yijing233.com/2020/02/21/WSL%E5%92%8Czsh%E7%BE%8E%E5%8C%96/install.png)](https://yijing233.com/2020/02/21/WSL和zsh美化/install.png)

在windows store 安装wsl，最好安装18.04或者16.04版，评价低的那个不推荐

[![img](https://yijing233.com/2020/02/21/WSL%E5%92%8Czsh%E7%BE%8E%E5%8C%96/store.png)](https://yijing233.com/2020/02/21/WSL和zsh美化/store.png)

然后就可以直接在搜索栏里打ubuntu，直接开始运行了

[![img](https://yijing233.com/2020/02/21/WSL%E5%92%8Czsh%E7%BE%8E%E5%8C%96/yunxing.png)](https://yijing233.com/2020/02/21/WSL和zsh美化/yunxing.png)

需要注意的是，子系统安装完的位置会在这个位置：

```
C:\Users\(用户名)\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs
```

### 2.安装zsh

终端输入

```
sudo apt-get install zsh
```

切换到 zsh，直接在终端输入 zsh 即可切换到 zsh。

安装 oh-my-zsh:

终端输入

```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

没啥问题就按y就完事了

### 3.美化环节

#### 修改主题

agnoster 主题在终端显示的效果比较暗，因此对主题文件修改一下

```
cp ~/.oh-my-zsh/themes/agnoster.zsh-theme ~/.oh-my-zsh/custom/themes/agnoster_wsl.zsh-theme
```

在这一步的时候出现了一点问题，所以直接copy了文件paste到相应的目录里（注意要改名字的！）

在终端键入 vim ~/.oh-my-zsh/custom/theme/agnoster_wsl.zsh-theme 对主题文件进行修改。在主题文件中找到：

```
prompt_dir() {
  prompt_segment blue $CURRENT_FG '%~'
}Copy
```

将 blue 修改为 075：

```
prompt_dir() {
  prompt_segment 075 $CURRENT_FG '%~'
}Copy
```

最后，在配置文件 ~/.zshrc 中修改主题为 agnoster_wsl：

```
vim ~/.zshrc
```

改成

```
ZSH_THEME="agnoster_wsl"
```

#### 隐藏终端前缀的用户名和主机名

在 ~/.zshrc 配置文件中添加：

```
DEFAULT_USER="用户名" # 引号的内容为你的用户名
```

#### 字体修改

只需要在终端顶部右键选择属性就可以

[![img](https://yijing233.com/2020/02/21/WSL%E5%92%8Czsh%E7%BE%8E%E5%8C%96/tizi.png)](https://yijing233.com/2020/02/21/WSL和zsh美化/tizi.png)

在这一步还有了新的问题，我的显示里是没有终端箭头的，目测是字体有点问题

[![img](https://yijing233.com/2020/02/21/WSL%E5%92%8Czsh%E7%BE%8E%E5%8C%96/terminal.png)](https://yijing233.com/2020/02/21/WSL和zsh美化/terminal.png)

需要更换为[Powerline字体](https://github.com/powerline/fonts)

我自己这边更换了Fira Code字体，显示就正常了

参考了[这个博客](https://www.jianshu.com/p/0effae21b862)的内容

[![img](https://yijing233.com/2020/02/21/WSL%E5%92%8Czsh%E7%BE%8E%E5%8C%96/change.png)](https://yijing233.com/2020/02/21/WSL和zsh美化/change.png)

都ok了，肥肠好看

### 参考资料

[WSL 使用 oh-my-zsh 让终端更美](https://www.jianshu.com/p/37f392355af1)

[WSL(Windows Subsystem for Linux)的安装与使用](https://www.cnblogs.com/JettTang/p/8186315.html)