# Manjaro

## 安装

manjaro官网下载iso文件，kde版本

Rufus官网下载，U盘制作工具

制作U盘，电脑调至UEFI，选择U盘启动

一步步安装即可

## 配置

### 镜像源配置

更换manjaro源

```shell
sudo pacman-mirrors -i -c China -m rank
```

执行完会弹出一个框，选择一个即可

更新数据源和系统

```shell
sudo pacman -Syyu
```

安装vim

```shell
sudo pacman -S vim
```

### 配置archlinux源

```shell
sudo vim /etc/pacman.conf
```

在最后添加

```shell
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

修改成功后，更新源和签名

```shell
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
```

## 常用软件安装

+ 搜狗输入法

```shell
sudo pacman -S fcitx-im
sudo pacman -S fcitx-configtool
sudo pacman -S fcitx-sogoupinyin
sudo vim ~/.xprofile
```

在配置文件中写入

```shell
export GTK_IM_MODULE=fcitx

export QT_IM_MODULE=fcitx

export XMODIFIERS="@im=fcitx"
```

+ Chrome浏览器

```shell
sudo pacman -S google-chrome
```

+ 网易云音乐

```shell
sudo pacman -S netease-cloud-music
```

+ WPS

```shell
sudo pacman -S wps-office
sudo pacman -S ttf-wps-fonts
```

+ vscode

```shell
sudo pacman -S visual-studio-code-bin
```

+ Git

```shell
sudo pacman -S git
```

+ yay

```shell
sudo pacman -S yay base-devel
```

+ wechat和tim

```shell

```

+ gimp

```shell
sudo pacman -S gimp
```

## 编程环境安装

### python

manjaro默认安装有python3

### go

```shell
sudo pacman -S go
```

### docker和docker-compose

```shell
sudo pacman -S docker
sudo pacman -S docker-compose
```

## 桌面美化

+ 安装 latte-dock

```shell
sudo pacman -S latte-dock
```

dock 栏确实方便，图标之类的其他配置自己调整即可
