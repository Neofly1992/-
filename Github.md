# Github 连接超时解决办法

因为某些不可抗力原因，国内访问 GitHub 经常失败，而很多软件的安装与配置依赖与此，这时我们有如下解决办法。

## gitee 官方备份

gitee.com 是国内比较大型的开源代码托管网站，因此我们可以借助其进行对 GitHub 的访问

我们以 oh-my-zsh 为例，其在 GitHub 上项目地址[在这里](https://github.com/robbyrussell/oh-my-zsh)，而其访问经常失败，这时我们可以使用 gitee.com 提供的[备份](https://gitee.com/mirrors/oh-my-zsh)，并在安装时替换对应地址即可。

## gitee 克隆仓库

如果 gitee 没有官方备份，则也可以自己手动备份。

在 gitee 上新建仓库，选择“导入已有仓库”，并输入对应项目地址，则可以将其克隆到 gitee 上，然后再 git clone 并离线安装即可。

## 修改 host

这是一个应对 DNS 污染较为通用的解决办法。

首先找到无法访问域名的正确 IP 地址，我们可以通过[这个网站](https://www.ipaddress.com/)或其他类似工具进行查询。

例如安装 [oh-my-fish](https://github.com/oh-my-fish/oh-my-fish) 时访问 raw.githubusercontent.com 失败，则查询结果如下：

```
raw.githubusercontent.com resolves to the following 4 IPv4 addresses:
185.199.108.133
185.199.109.133
185.199.110.133
185.199.111.133
```

然后我们去编辑 host 文件（建议编辑前先备份），命令为：

```
sudo vim /etc/hosts
```

在其中添加如下字段：

```
# GitHub
185.199.108.133 raw.githubusercontent.com
185.199.109.133 raw.githubusercontent.com
185.199.110.133 raw.githubusercontent.com
185.199.111.133 raw.githubusercontent.com
```

保存后重新下载，可以发现访问成功。在安装等工作完成后，别忘了将 host 文件恢复即可。

