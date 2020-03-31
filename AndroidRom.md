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