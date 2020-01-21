---
layout: post
title: 从终端开始优化 GitHub Pages 博客写作环境
categories: [blog ]
tags: [ Guide, Mac, ]
description: "如何快速创建博客模板文档开始写作，以及在博客写作完成后快速发布的心得"
image:
  feature: windows.jpg
  credit: Azeril
  creditlink: azeril.com
---


以 Jekyll 为主的 GitHub Pages 博客是有它非常让人充满好感的方面，比如维护成本低、安全、多平台

通过命令行将日常博客使用中的一些繁琐操作半自动化，可以便捷到什么地步，本篇希望能梳理出一个概念还有一条优化的思路来。

## 快速进入写作环境


日常，一般情况下使用 GitHub Pages 博客都不免是这样的流程：

1. Finder 上点点点，打开博客仓库；
2. 进入 _posts 目录， 复制一个文档作为新博文文档使用；
3. 清理文档，删除博文内容，修改 YAML；
4. 开始写作。


或者：

1. 打开编辑器，开始写作；
2. 保存文档，将文档移动到对应的博客仓库 _posts 目录；
3. 打开 Finder，进入仓库 _posts 目录;
4. 复制文档 YAML 信息，粘贴到新博文并修改；

如果借助命令行工具创建的脚本，可以让整个步骤变得非常迅捷。像这样：

1. 唤出 iTerm，输入短命令.进入命令自动执行状态。自动在仓库中完成附带 YAML 信息的模板文档的创建，同时一并打开 _posts 目录，以及用个人自定义的 Markdown 编辑器打开新创建的模板文档。
2. 在自动弹出的编辑器界面，开始写作；
3. 修改 YAML 信息和文档标题（只涉及日期字段后的 Title 部分），保存。

命令集(引号中的部分，可作为 alias 短命令添加)：

- 使用 Sublime Text 作为默认编辑器：

```
cd ~/Your_blog_Path/_posts/ ; open . ; cp Template.md ` date +20%y-%m-%d`-Title.md ; sleep 0.3s ; open -a MacDown ` date +20%y-%m-%d`-Title.md
```

- 使用 MacDown 作为默认编辑器：

```
cd ~/Your_blog_Path/_posts/ ; open . ; cp Template.md ` date +20%y-%m-%d`-Title.md ; sleep 0.3s ; Sublime ` date +20%y-%m-%d`-Title.md
```

使用时修改第一条 cd 命令的路径，并为当前 GitHub Pages 博客仓库的 _posts 目录中添加一个 YAML 信息完整的博文文档模板 `Template.md`，作为创建新文档的母版。

## 快速推送博文更新

GitHub 是非常棒的工具，通过 GitHub 的客户端、命令行和网页版版，现在我们能越加方便地使用基于 GitHub 的各种功能。

当然，在追求效率的时候，还是可以对于日常操作进行优化的：

- 使用客户端的方案：

1. 打开 GitHub Desktop，切换到对应博客仓库；
2. 添加更新描述，而后确认提交（桌面版在操作上简化了添加的动作，只保留了提交）；
3. 点击同步，推送更新；


- 使用 Git 的方案：

1. 打开终端；
2. 进入对应的仓库；
3. 分步输入 Git 命令：$git add，添加改动；
4. $git commit，提交改动；
5. $git push，推送更新。

那简化方案是如何的呢？一条短命令（被固化为短命令的命令集），合成「添加-提交和推送」等操作，一步到位。在设定完成后，涉及到的操作只有一个：

1. 打开终端，输入短命令，点击 Return 完成所有上述操作。

而实际上，这样一个短命令所能做的操作比另两个方案诸多操作叠加还多。

大致来说是依次完成了这些动作：

- 进入博客仓库目录
- 添加更新
- 提交更新
- 推送更新
- 暂定 0.5s（或其它自定时刻）
- 切换到浏览器，加载博客主页

终端任意目录为博客仓库执行快速更新命令集：

- 添加新博文:

```
$cd ~/Your_Blog_Path && git add . ; git commit -m "Add a new post" ; git push ; sleep 0.5s ; open Your_blog_Website 
```

添加为 alias 短命令：

```
# 添加新博文
alias pst="cd ~/Your_Blog_Path && git add . ; git commit -m 'Add a new post' ; git push ; sleep 0.5s ; open 'Your_blog_Website'" 
```

- 更新博客设定:

```
cd ~/Your_Blog_Path && git add . ; git commit -m "update the files" ; git push
```

添加为 alias 短命令：

```
# 更新博客设定
alias blg="cd ~/Your_Blog_Path && git add . ; git commit -m 'update the files' ; git push" 
```

在任意仓库内执行快速更新命令集：

添加为 alias 短命令：

```
alias nuts="git add . ; git commit -m 'update the files' ; git push"
```


最终，在设定完成后，使用短命令创建博客文档和推送博客更新的呈现效果：


![QuickStartGuideForBlogWriting](img/swirl/VideoQuickStartGuideForBlogWriting.gif)

## 注释

为方便理解，对于以上的命令集中几个命令做一简要注释。

- cp 基本命令，用于复制文档，可在复制操作的同时更改新文档（被复制出来的文档 file_2）名称：`$cp file_1 file_2`。
- open 用于打开 Finder 和 url。这篇中使用了 `open -a Application_name file_name``$open .` 和 `$open url` 3 种，分别对应用用某个软件在桌面上打开某个文档，用Finder 打开终端当前所处的目录和用（默认）浏览器打开网址。 
- date 命令用于生成时间。`$date +%y%m%d `，在终端中打印出（6 位数）当前日期。稍作修改即可作为 Jekyll 博客专用的年月日加标题格式 Markdown 博文文档。
- sleep 延迟命令，用于推迟后续操作的执行时间。`sleep number(unit)` 进行设定，数字后不跟单位，默认时间单位为秒。
- vim 命令的使用，vim 的命令这里不多说了。
- alias 添加 alias 很简单，alias Short_CMD="Long_CMD"。  
- source 让 bash 配置生效的操作。在退出 vim 界面返回终端主界面时，执行 `source ~/.bashrc` 或 `source ~/.zshrc`（如果你使用的是 zsh 或 oh my zsh）。


