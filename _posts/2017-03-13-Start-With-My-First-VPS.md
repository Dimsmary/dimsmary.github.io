---
layout: post
title: VPS 初相遇
categories: [blog ]
tags: [Coding, Linux, ]
description:
image:
  feature: windows.jpg
  credit: Azeril
  creditlink: azeril.com
---


## 为什么使用 VPS

其实购置 VPS 的念想是由来已久的。至于为何过去几年里最终都没动手，更多只能归结为懒...

VPS 的用途广泛，小到作为同步的云盘、小型博客，大到布局网站、下载站、应用服务托管等，都能顺应自如。至于说是什么促成了我这一回下单，更多是出于熟悉 Linux 服务器的操作，为后边捋顺从开发倒部署上线的整个流程打下一点基础。日常的话，也可以多一个除了 GitHub 之外的试验场。


## VPS 的选购

VPS 服务的网络服务商不少，按照自己的喜好和需求来即可。


就选购时的衡量来说，单纯自用也最好是满足两个条件，配置相对好（符合需求的前提），价格尽量低，听起来怎么像不给吃草还让疯跑的想法？但事实如此，稍微挑选一下，还是能找到合适的套餐的，合适就好。这次是选购了 [Bandwagonhost](https://bandwagonhost.com) 的。另外有一家 [Vultr](https://www.vultr.com/) 也不错。

国内的主机或弹性云计算服务器与这一类 VPS 类似，诸如阿里云、百度云或腾讯云都在提供相应的服务。就价格而言，国内的服务器售价更高，优势是服务器布局在中国，国内访问会更好。当然，就实际的使用我发现搬瓦工的网络还不赖，进行操控和传输，参数都很可观，甚至于用这个 VPS 搭建的私人 Shadowsocks 服务，自用体验也还可以。

## 初步配置

- 安装系统
- 一键安装 Shadowsocks 服务

这些操作其实没啥好说的，BandwagonHost 提供了很简明的操作面板，根据菜单的选项操作就能很容易完成系统的安装或切换。

![BandwagonHostKiwiVMUI](/img/swirl/BandwagonHostKiwiVMUIInstallOS.png)

需要注意的一点是，一键安装 Shadowsokcs 服务，只支持 CentOS 6 的版本（默认安装的服务器操作系统也是这个），安装其它的版本或其它 Linux OS 一键安装服务会失效。至于选择什么操作系统，这事见仁见智。以前在个人电脑上安装的虚拟机或实体 Linux 系统一直都是 Ubuntu。但这次为了偷懒，直接可以不折腾就部署好 Shadowsocks 服务，就直接使用了以前从来没接触过的 CentOS，操作起来也还不赖。当然，没有图形界面，这个没办法，甚至都不好意思吐槽，毕竟各种操作，我想不出比命令行更方便的途径了...


## SSH 登录 VPS

虽然搬瓦工提供了网页版的终端方便操作，当相比 macOS 下的 iTerm 来说，便利性还是相差太多了。通过 SSH 与 VPS 进行连接，能很大程度为 VPS 的操控提供便利——这样就又能使用 iTerm 和 zsh 一类的神器了。

初次使用 SSH 登录新购置的 VPS 时，耗了不少时间在登录命令上。这个跟头摔的有点猝不及防...

初翻了几篇教程，都没细说 SSH 登录的命令格式。自己找了一下，格式是这样：`ssh <username>@<Serverdomain/IP> -p <port>`

先前为了力求个性化，毕竟第一次购置 VPS，也是蛮值得纪念的一件事，便顺手将主机名(Hostname)改成了 stardust，一部我非常喜欢的小说的名字。然后，惯性使然，加之控制面板那边又的确是明确说了这是 Hostname，在使用 SSH 的时候我也就想当然地将自己新冠名的主机名作为 username 填上去了。接下来就是几个小时的僵持和我的好一阵烦躁...数次在控制面板修改 root password，数次试图通过切换 Linux 系统或版本来绕过一次次在用 SSH 链接时，输入密码确认，每一次都是错误（Permission denied），每累积三次错误就关闭 SSH 访问请求...购置 VPS，开启自己个人 Shadowsocks 服务的喜悦还没来得及在脸上舒展开就被这一阵折腾雨打风吹去...

隔了一晚，今天中午时再试，总想求得一个结果啊。这回长了心眼，重新审视了一下登录用的命令。后边再扩大关键词，搜索更多相关问答翻找了好多才在 V 站找到了破题点——[《SSH 无法登陆， SFTP 正常 - V2EX》](https://www.v2ex.com/t/224479)。实际上用户名对应位置应该填写的就是 root 而不是自己创建的 hostname，填 root 就是这么简单粗暴...

当然这个不同主机估计会有差异，这里只针对搬瓦工运营的 VPS。

改好之后就通过远程访问，确认密码后顺利进入系统...泪目。

## 为 VPS 配置公共密钥

解决了 SSH 登录的问题，后边就主要看自己想怎么个瞎折腾了。估计网站博客应用什么的都会一点点玩起来。不过现在还是先解决一个日常访问主机时需要面对的问题。每次 SSH 都需要输控制面板随机生成的密码，输入麻烦不说，自己又记不住，在找密码复制粘贴中平白浪费不少功夫。

先前看的各种教程中有提及使用公共密钥免密码用 SSH 访问 VPS 的。就很来劲，id_rsa.pub 这个公共密钥老早在配置 Git 访问 GitHub 时就生成过，既然是现成的这就好办了，无非是将密钥上传到服务器对应的位置（以及修改一下命名），完成对应设定就好。

生成公共密钥的操作这里不赘述。主要讲一下密钥生成之后，将其添加到服务器的整个流程。

1. SSH 登录，在 VPS 主目录下，`mkdir .ssh` 创建一个 `.ssh` 的隐藏目录，用于放置密钥；
2. 终端切换到本地的系统主目录，用 `scp` 命令将生成好的公钥上传到 CentOS 的 `.ssh` 目录下，并将公钥重命名为 authorized_keys：
`scp -P Port_Number ~/.ssh/id_rsa.pub root@VPS_IP:~/.ssh/authorized_keys`，确认密码后完成上传；
4. 修改服务器端文件与文件夹的权限：

```
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/id_rsa 
```

## 优化 VPS 的登录

通过 SSH 访问私人服务器，除却需要费劲输入主机名、端口和老长一串 IP，多少也是记忆上的负担。这时，可以使用 alias 来为登录用的 SSH 制作简单好记的短命令，方便登录。这里设定对应的短命令为 vps：

```
alias vps="ssh -p 29240 root@138.128.205.122"
```

设置完成后，进行部署使之生效：

```
source ~/.bashrc
```

如果安装了 zsh 或 oh my zsh，则：

```
source ~/.zshrc
```

设定生效之后，以后，打开 Termial，输入 vps 回车就可以直接 SSH 登录 VPS 开始玩耍了。

## 参考资料

- [VPS 有什么有趣的用途？ - 知乎](https://www.zhihu.com/question/24284566)
- [HowTos/Network/SecuringSSH - CentOS Wiki](https://wiki.centos.org/HowTos/Network/SecuringSSH)
- [17.5. OpenSSH Configuration Files](https://www.centos.org/docs/5/html/Deployment_Guide-en-US/s1-ssh-configfiles.html)
