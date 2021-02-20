#  Manjaro 系统安装与配置

## Manjaro 系统安装

### 版本下载

首先去 [Manjaro](https://manjaro.org/) 官网下载自己喜欢的版本，官方提供了多种桌面环境，我个人还是更喜欢 GNOME 版本。此外还提供 XFCE 与 KDE 桌面环境的，也有 CLI 命令行的，可以根据需要自行下载。

### 制作启动 U 盘

下载之后使用 [rufus](https://rufus.ie/) 将 ISO 文件写入 U 盘中。看到网上很多教程都在反复强调，这里需要使用 dd 模式写入，否则会有引导问题。但是事实上新版的 rufus 已经没有这个选项了，直接写入即可。

### 系统安装

因为我实在是对于每次重装系统后都需要重新配置引导烦了，所以索性直接买了块硬盘，将 Manjaro 直接装在新硬盘里，这样无论是安装过程中对于分区的配置与后面引导项的修改都简单一点。

因此这里没有对此部分的多余描写，有兴趣可以在网上搜索其他教程，自行解决。

## Manjaro 系统配置

### 换源

进入系统后，输入：

```
sudo pacman-mirrors -i -c China -m rank
```

在弹出的对话框中，选择自己喜欢的源。这里推荐清华和中科大的。

接着手动添加 archlinuxcn 的源：

```
sudo nano /etc/pacman.conf
```

在文件末尾输入：

```
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch3
```

保存退出后，根据自己需要选择 Server，这个和之前的选择没有关系。

然后更新系统：

```
sudo pacman -Syyu
```

更新好之后，安装 archlinuxcn-keyring：

```
sudo pacman -S archlinuxcn-keyring
```

再更新一次系统：

```
sudo pacman -Syyu
```

系统更新完成。

### 安装 AUR

[AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 是 Arch Linux 社区用户软件仓库（ArchLinux User-community Repository）的缩写，类似 Debian/Ubuntu 的 PPA。它是一个社区仓库，里面包含了很多没有被官方背书的软件，数量很大，且更新比较快，也是 Arch Linux 的一大优势。

Arch 的包管理器 是 [pacman](https://wiki.archlinux.org/index.php/Pacman_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))，它并不直接支持 AUR，因此需要使用 [AUR助手](https://wiki.archlinux.org/index.php/AUR_helpers_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))。

在过去的很长时间，Yaourt （Yet AnOther User Repository Tool）是很多人的选择，它是 `pacman` 的一个封装，基本上采用和 `pacman` 一样的语法，因此其对于 AUR 的搜索、安装、乃至冲突解决和包依赖关系维护都有着良好的支持。然而，Yaourt 的开发进度近来十分缓慢，甚至在 Arch Wiki 上已经被列为“停止或有问题”。

因此很多人选择了新的[替代品](https://linux.cn/article-12019-1.html)。目前我使用的是 [yay](https://github.com/Jguer/yay)，它是用 Go 语言编写的，简单易用，使用方法同样和 `pacman` 很相似。

它的安装方法非常简单：

```
sudo pacman -S yay
```

以后安装很多官方仓库没有收录的软件时，就可以使用它了。

### 安装输入法

对于新安装的 Linux 系统，很多时候没有输入法是很头疼的一件事，需要尽早安装。

目前主流的选择是[搜狗输入法](https://pinyin.sogou.com/linux/?r=pinyin)，不过其实也有比较多的问题，因为它使用的是 `fcitx` 框架，其最新版本已经更新到了 qt5，而搜狗还在使用 qt4 的框架，因此需要单独安装 fcitx-qt4，且其和 fcitx-im 不兼容，只能选择使用一个。这种教程网上也有很多，我这里就不赘述了。

我自己使用的是 [rime](https://rime.im/)，不过说实话这东西更新的也挺缓慢的了。它也存在 fcitx 框架的版本，不过应该不是官方维护的，所以这里我们采取 ibus 的版本。安装方法如下：

```
sudo pacman -S ibus-rime ibus-table ibus
```

Arch Linux 下已自带双拼方案，无需单独安装。

安装完成后需要修改一下输入法配置文件：

```
gedit ~/.config/ibus/rime/default.custom.yaml
```

这里可以按下面的方式配置候选语言：

```
patch:
  schema_list:
    - schema: luna_pinyin          # 朙月拼音
    - schema: luna_pinyin_simp     # 朙月拼音 简化字模式
    - schema: luna_pinyin_tw       # 朙月拼音 臺灣正體模式
    - schema: terra_pinyin         # 地球拼音 dì qiú pīn yīn
    - schema: bopomofo             # 注音
    - schema: jyutping             # 粵拼
    - schema: cangjie5             # 倉頡五代
    - schema: cangjie5_express     # 倉頡 快打模式
    - schema: quick5               # 速成
    - schema: wubi86               # 五笔 86
    - schema: wubi_pinyin          # 五笔拼音混合輸入
    - schema: double_pinyin        # 自然碼雙拼
    - schema: double_pinyin_mspy   # 微軟雙拼
    - schema: double_pinyin_abc    # 智能 ABC 雙拼
    - schema: double_pinyin_flypy  # 小鶴雙拼
    - schema: wugniu               # 吳語上海話（新派）
    - schema: wugniu_lopha         # 吳語上海話（老派）
    - schema: sampheng             # 中古漢語三拼
    - schema: zyenpheng            # 中古漢語全拼
    - schema: ipa_xsampa           # X-SAMPA 國際音標
    - schema: emoji                # emoji 表情
```

我自己只使用 `朙月拼音` 和 `小鶴雙拼`，其他的语言注释掉或者删除即可。

然后我们再配置 `朙月拼音` 的文件：

```
gedit ~/.config/ibus/rime/luna_pinyin.custom.yaml
```

导入如下配置：

```
# luna_pinyin.custom.yaml

patch:
  switches:                   # 注意缩进
    - name: ascii_mode
      reset: 1                # reset 0 的作用是当从其他输入法切换到本输入法重设为指定状态
      states: [ 中文, 西文 ]   # 选择输入方案后通常需要立即输入中文，故重设 ascii_mode = 0
    - name: full_shape
      states: [ 半角, 全角 ]   # 而全／半角则可沿用之前方案的用法。
    - name: simplification
      reset: 1                # 增加这一行：默认启用「繁→簡」转换。
      states: [ 漢字, 汉字 ]
```

### 将目录修改为英文

Manjaro 的 路径默认是中文的，在终端打字切换输入法很麻烦且浪费时间，因此建议将路径设置为英文，方式如下：

```
sudo pacman -S xdg-user-dirs-gtk
export LANG=en_US
xdg-user-dirs-gtk-update
#然后会有个窗口提示语言更改，更新名称即可
export LANG=zh_CN.UTF-8
#然后重启电脑如果提示语言更改，保留旧的名称即可
```

### 双系统时间同步

对于像我这种双系统的情况，每次切换系统后会出现[系统时间](https://wiki.archlinux.org/index.php/System_time_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))混乱的问题。其主要原因是，Windows 使用的是 localtime，而Linux 是 UTC。前者依赖当地失去，后者则是采用格林威治时间。

因此我们可以将 Windows 设置为 UTC，或者将 Linux 设置为 localtime。这里我们选择后者：

```
sudo timedatectl set-local-rtc true
```

### 安装 oh-my-zsh

Mannjaro默认自带 oh-my-zsh，不需要单独安装。如果像使用的话，首先要先把默认 shell 改为 zsh：

```
chsh -s /usr/bin/zsh
```

再安装 oh-my-zsh：

```
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```

不翻墙的话，这个下载很慢，有失败的可能行。

### 其他软件安装

```
sudo pacman -S wine
sudo pacman -S vim
sudo pacman -S emacs
sudo pacman -S google-chrome
yay -S visual-studio-code-bin
yay -S netease-cloud-music
yay -S baidunetdisk
sudo pacman -S typora
sudo pacman -S calibre
sudo pacman -S shadowsocks-qt5
sudo pacman -S make
sudo pacman -S clang
sudo pacman -S gdb
sudo pacman -S mpv
sudo pacman -S vlc
sudo pacman -S obs-studio
sudo pacman -S motrix
sudo pacman -S vmware-workstation
sudo pacman -S android-tools
sudo pacman -S codeblocks
sudo pacman -S steam
sudo pacman -S albert
sudo pacman -S indicator-sysmonitor
sudo pacman -S guake // kde 桌面环境可以使用 yakuake
sudo pacman -S fish // 可以使用 oh-my-fish 作为配置文件，并安装插件如 wttr 等
```

### 字体安装

首先安装一些字体：

```
sudo pacman -S ttf-roboto noto-fonts ttf-dejavu
sudo pacman -S wqy-bitmapfont wqy-microhei wqy-microhei-lite wqy-zenhei
sudo pacman -S noto-fonts-cjk adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts
sudo pacman -S otf-fira-code
sudo pacman -S nerd-fonts-complete
```

然后创建文件 `~/.config/fontconfig/fonts.conf`，并导入以下配置：

```
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">

<fontconfig>

    <its:rules xmlns:its="http://www.w3.org/2005/11/its" version="1.0">
        <its:translateRule translate="no" selector="/fontconfig/*[not(self::description)]"/>
    </its:rules>

    <description>Manjaro Font Config</description>

    <!-- Font directory list -->
    <dir>/usr/share/fonts</dir>
    <dir>/usr/local/share/fonts</dir>
    <dir prefix="xdg">fonts</dir>
    <dir>~/.fonts</dir> <!-- this line will be removed in the future -->

    <!-- 自动微调 微调 抗锯齿 内嵌点阵字体 -->
    <match target="font">
        <edit name="autohint"> <bool>false</bool> </edit>
        <edit name="hinting"> <bool>true</bool> </edit>
        <edit name="antialias"> <bool>true</bool> </edit>
        <edit name="embeddedbitmap" mode="assign"> <bool>false</bool> </edit>
    </match>

    <!-- 英文默认字体使用 Roboto 和 Noto Serif ,终端使用 DejaVu Sans Mono. -->
    <match>
        <test qual="any" name="family">
            <string>serif</string>
        </test>
        <edit name="family" mode="prepend" binding="strong">
            <string>Noto Serif</string>
        </edit>
    </match>
    <match target="pattern">
        <test qual="any" name="family">
            <string>sans-serif</string>
        </test>
        <edit name="family" mode="prepend" binding="strong">
            <string>Roboto</string>
        </edit>
    </match>
    <match target="pattern">
        <test qual="any" name="family">
            <string>monospace</string>
        </test>
        <edit name="family" mode="prepend" binding="strong">
            <string>DejaVu Sans Mono</string>
        </edit>
    </match>

    <!-- 中文默认字体使用思源宋体,不使用 Noto Sans CJK SC 是因为这个字体会在特定情况下显示片假字. -->
    <match>
        <test name="lang" compare="contains">
            <string>zh</string>
        </test>
        <test name="family">
            <string>serif</string>
        </test>
        <edit name="family" mode="prepend">
            <string>Source Han Serif CN</string>
        </edit>
    </match>
    <match>
        <test name="lang" compare="contains">
            <string>zh</string>
        </test>
        <test name="family">
            <string>sans-serif</string>
        </test>
        <edit name="family" mode="prepend">
            <string>Source Han Sans CN</string>
        </edit>
    </match>
    <match>
        <test name="lang" compare="contains">
            <string>zh</string>
        </test>
        <test name="family">
            <string>monospace</string>
        </test>
        <edit name="family" mode="prepend">
            <string>Noto Sans Mono CJK SC</string>
        </edit>
    </match>

    <!-- 把Linux没有的中文字体映射到已有字体，这样当这些字体未安装时会有替代字体 -->
    <match target="pattern">
        <test qual="any" name="family">
            <string>SimHei</string>
        </test>
        <edit name="family" mode="assign" binding="same">
            <string>Source Han Sans CN</string>
        </edit>
    </match>
    <match target="pattern">
        <test qual="any" name="family">
            <string>SimSun</string>
        </test>
        <edit name="family" mode="assign" binding="same">
            <string>Source Han Serif CN</string>
        </edit>
    </match>
    <match target="pattern">
        <test qual="any" name="family">
            <string>SimSun-18030</string>
        </test>
        <edit name="family" mode="assign" binding="same">
            <string>Source Han Serif CN</string>
        </edit>
    </match>

    <!-- Load local system customization file -->
    <include ignore_missing="yes">conf.d</include>
    <!-- Font cache directory list -->
    <cachedir>/var/cache/fontconfig</cachedir>
    <cachedir prefix="xdg">fontconfig</cachedir>
    <!-- will be removed in the future -->
    <cachedir>~/.fontconfig</cachedir>

    <config>
        <!-- Rescan in every 30s when FcFontSetList is called -->
        <rescan> <int>30</int> </rescan>
    </config>

</fontconfig>
```

