# 刷机记录

这几天心血来潮，把已经不用的小米 4 翻了出来，他之前被我刷成砖了，不过其实硬件还是好用的，我们试试把它强刷回来。

*这部分内容基本抄自魔趣*

## 解锁

首先第一步是解 BL 锁。需要指出的是，一部手机解锁一次就可以了，以后再刷机不需要重新解锁。

先介绍以下什么是 BL 锁：BL 是 bootloader 的简称 就是开机引导程序 。Bootloader 锁，主要是在引导过程中对系统签名，内核签名及 Recovery 签名进行检验。如果签名不一致，即终止引导。它是限制用户刷第三方 ROM 和第三方 recovery，以及限制 root 的“锁”（我们所说的“解锁”就是他），锁住 recovery 和 fastboot 不会被其他东西随意刷机和篡改。BL 未解开状态下无法 root 也无法刷第三方 ROM。因为刷第三方 ROM 就必须要刷入第三方 REC。

下面介绍以下具体步骤：

1. 登录 MIUI [解锁 BL 的官网](https://www.miui.com/unlock/)；
2. 按照网站的指导，进行申请与工具下载等操作；
3. 进入“设置 -> 开发者选项 -> 设备解锁状态”中绑定账号和设备；
4. 手动进入Bootloader模式（关机后，同时按住开机键和音量下键）；
5. 通过USB连接手机，点击 “解锁”按钮；
6. 如果还不明白可以查看 [FAQ](https://www.xiaomi.cn/post/17982230)

## 安装 Recovery

Recovery 是 Android 手机备份功能，指的是一种可以对安卓机内部的数据或系统进行修改的模式（类似于 windowsPE 或 DOS）。在这个模式下我们可以，对已有的系统进行备份或升级，也可以在此恢复出厂设置。 刷入第三方的 Recovery，将获得更多的功能，并且可以刷入第三方 rom，官方自带则不行。

安装流程如下：

1. 下载 Recovery。例如 [TWRP](https://twrp.me/)。

2. 通过 USB 将您的设备连接到电脑。

3. 通过USB将您的设备连接到电脑。

4. 重启进入fastboot 模式。一般可以采用以下两种方法：

   - 在电脑上打开命令提示符（Windows）或 终端 （Linux 或 macOS）并输入 `adb reboot bootloader`。

   - 或者通过组合键方式。即关闭设备后，按住音量调低 + 电源键，直到屏幕上方出现 “FASTBOOT” 字样，然后松开。

5. 一旦设备处于 fastboot 模式，请通过键入 `fastboot devices` 验证您的 PC 是否找到它。

   *如果在 Linux 或 macOS 上看到 "no permissions fastboot"，请在 ROOT 下运行该命令*

6. 使用命令 `fastboot flash recovery twrp-x.x.x-x-x.img` 将 Recovery 刷入到您的设备。

   *请根据 Recovery 的名称对命令做相应的调整*

7. 关闭设备后，按住音量调高 + 电源键，直到进入 Recovery 模式，然后松开，以验证安装。

## 安装 ROM

1. 下载你想要安装的 ROM 包。

   *这里的 ROM 不仅包括各种第三方 ROM，也包括一些扩展包，诸如 [Open GApps](https://opengapps.org/) 等*

2. 关闭设备后，按住音量调高 + 电源键，直到进入 Recovery 模式，然后松开。

3. 点击主界面的 WIPE 按钮。

4. 点击 Format Data 执行格式化过程。这将删除加密以及存储在内部存储上的所有文件。

5. 定位到您存储在内部存储上的 ROM 包，并执行安装过程。

6. 使用同样的方式安装其它第三方扩展包。

7. 安装结束后，返回主菜单，点击 Reboot, 然后点击 System。

## Termux

Termux 是一个 Android 下一个高级的终端模拟器,开源且不需要 root，支持 apt  管理软件包，十分方便安装软件包，完美支持 Python、 PHP、 Ruby、 Nodejs、  MySQL等。随着智能设备的普及和性能的不断提升，如今的手机、平板等的硬件标准已达到了初级桌面计算机的硬件标准，用心去打造 DIY  的话完全可以把手机变成一个强大的极客工具。

需要指出的是，如果是使用酷安应用市场安装的话，可能会在启动时提示 “Termux was unable to install the bootstrap packages”，如果这时又不方便挂代理的话，可以选择去 F-Droid 平台下载。事实上现在官网中的 Release 也是 “F-Droid only”。

1. 下载 [Termux](https://f-droid.org/packages/com.termux/) 并安装。

2. 软件安装。Termux 在 apt 的基础上封装了 pkg 命令，并且向下兼容 apt 命令，因此使用起来非常方便。可以修改镜像为[清华源](https://mirror.tuna.tsinghua.edu.cn/help/termux/)以提高速度。

   *Termux 的家目录并不在 /home 下，而是在 /data/data/com.termux/files/home。因此在修改配置时需要注意。Termux也为我们预定义了 `$HOME` `$PREFIX` `TMPPREFIX` 等变量方便使用。*

3. 定制按键。编辑按键配置文件 `vim ~/.termux/termux.properties`，输入如下内容：

   ```
   extra-keys = [ \
    ['ESC','|','/','HOME','UP','END','PGUP','DEL'], \
    ['TAB','CTRL','ALT','LEFT','DOWN','RIGHT','PGDN','BKSP'] \
   ]
   ```

   重启生效。

4. 超级管理员身份。

   1. 手机未 root

      利用 proot 可以为手机没有 root 的用户来模拟一个 root 的环境，该环境模仿 Termux 中的常规Linux文件系统，但是不是真正的 root。

      安装 proot 后，可以通过命令 `termux-chroot` 模拟 root 环境。退出时可以使用 `exit` 命令。

   2. 手机已 root

      可以使用 tsu，其是一个 su 的 Termux 版本。是一个真正的root权限，用来在 termux 上替代 su，操作不慎可能对手机有安全风险。

      同样使用 `tsu` 命令切换到 root 用户，使用 `exit` 命令退出。

5. 端口查看。Android 10 及以下的版本可以正常使用 `netstat -an` 命令，目前 Android 10 以上版本上述命令存在问题，可以安装 nmap 后使用 `nmap 127.0.0.1` 命令替代。

