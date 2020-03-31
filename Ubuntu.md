# Ubuntu 安装小记

*这是 2017 年的时候，在笔记本上安装 Ubuntu 时遇到问题并解决的方法，不过目前本人已投靠 Manjaro……此文只是搬运过来，不再更新*

## 安装系统

在 Ubuntu17.10 发布了将近一个月之后，我才看到了这个消息。这个版本更新的内容甚是不少，~~再说我也只是用来装逼的而已，~~没有理由不赶紧装一个尝尝鲜吧。但是平均一年才装一次的我，果然又顺理成章的忘了这东西怎么搞了，今天写点随笔，也权当是个备忘录吧。
首先是分区的分配。这次吸取了上次的教训，`/boot`分区只有 200M 虽说是够用了，但是每次更新都得卸载内核，三天两头提示空间不够。这次索性分 1G 再说；`swap`还是老样子的等于物理内存的大小；这次多分了个`/home`，除了`/`分了 10G 外都留给它了，权且当个试验吧。
其次是启动引导器的安装路径。 说实话我忘了上次安在哪里了……我是这么想的：一是 Win10 安在固态上了，Ubuntu 就不去固态凑热闹了；二是既然有个单独的`/boot`分区，我就把启动引导器安在这个启动分区上吧，结果上看起来好像没啥毛病。
最后自然就是老生长谈的`grub rescue`了。果然不出所料，这次又进了……百度了下，基本上解决办法如下：

1. `grub rescue`的常用指令主要有`set`，`ls`，`insmod`，`root`和`prefix`这五种，具体就不挨个介绍了，反正下面都用得到；
2. 先通过`ls`指令查看硬盘分了多少个区，一般都是形如（hd0,msdos1)这样的，注意这里的“0"取决与你有多少个硬盘，后面的”1"取决你分了多少区，也就是说这里的数字是不固定的~~(废话，要不然说“列出来多少个”……）~~；
3. 对于每个分区，我们可以尝试`ls (hd1,msdos7)/grub`这样的指令对其进行穷举，最后找到 Ubuntu 所在的分区。这里需要注意的是，根据安装时分区方式的不同，可能要对后面的指令有所修正，比如将`/grub`换成`/boot/grub`。这里的“1"与”7"是我个人这次实际试出来的值；
4. 接下来就是实际的设置了。有如下三条指令：
```
set=(hd1,msdos7)
prefix=(hd1,msdos7)/grub
insmod normal
```
这里需要说明两点：其一是第二条指令有的资料写的是`prefix=(hd1,msdos7)`，但是我个人的经验是并没有卵用；其二是还有的资料对第三条指令写的是`insmod /boot/grub/normal.mod`，同样的针对有单独`/boot`分区的不需要打`/boot`。不过我并没有尝试过，有机会下次再试吧~~（希望没这个机会……）~~；
5. 输入`normal`，这样就可以启动了；
6. 别高兴的太早，这只是临时的，你再重启又进`grub rescue`了……想要彻底解决这个问题需要修复 grub，这里我们先使用`sudo fdisk -l`查看一共有多少分区(我这里是`/dev/sdb7`），我们将`/boot`放在了哪个分区，找到它，再使用`sudo update-grub2`与`sudo grub-install /dev/sda`对其进行修复。这里需要注意的是，其中输入的是硬盘的编号，所以不要在后面加什么`sdb7`之类的。不过我不太明白的一点是为什么我明明 Ubuntu 安装在 sdb7 上，但是这里需要修复到 sda 中。
7. 启动方面最后需要解决的就是默认启动项没有 Windows，这里我们可以采用`sudo gedit /boot/grub/grub.cfg`命令，将`set default`项的值修改为“4"即可。
8. 经过以上这么多部，系统应该就可以正确进入了。最后一个问题就是双系统时间的修改了。这个是由于 UTC 的问题导致的，不赘述了。这里我们默认使用的是16.04及以后的版本。可以使用这条命令：`timedatectl set-local-rtc 1 --adjust-system-clock`，之后重启 Ubuntu 就可以了。当然也可以修改 Windows，不过安全起见没必要自找麻烦，折腾的活全留在 Ubuntu上吧。
至此，基础安装的过程就基本完毕了。Enjoy your system！

## 常用软件及 PPA

**可以多参考“https://www.ubuntuupdates.org”网站进行查看**

Rime
```
sudo add-apt-repository ppa:lotem/rime
sudo apt-get update
sudo apt-get install ibus-rime
sudo apt-get install librime-data-double-pinyin
sudo ibus-setup 
```

Typora
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
sudo add-apt-repository 'deb http://typora.io linux/'
sudo apt-get update
sudo apt-get install typora
```

Chrome

```
sudo wget https://repo.fdzh.org/chrome/google-chrome.list -P /etc/apt/sources.list.d/
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
sudo apt-get update
sudo apt-get install google-chrome-stable
```