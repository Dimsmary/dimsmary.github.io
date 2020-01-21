---
layout: post
title: 使用代理，突破 Google Scholar 访问受限
categories: [blog ]
tags: [Mac, Google,]
description: "解决使用 Google 学术搜索被 Google 屏蔽/封锁的问题"
image:
  feature: windows.jpg
  credit: Azeril
  creditlink: azeril.com
---

最近遇到好几回同事 Google 搜索服务能正常使用，而 Google Scholar 无法访问的事。先前用 Google 检索了不少的网络记录，费了一阵折腾，换 SS 服务器、修改 Hosts（包括添加 Google Scholar 的 IPV6 地址）、清空 DNS 缓存等，几番尝试都没搞定，今天也是一样，因为我都忘了上次最终怎么搞定的了...当然，最后是回溯了一下多年来与糟糕的网络状况相伴，以及在此间的种种解决方法，最终还是搞定了。

只能暗自感慨一下，真不长记性，是为一记。

![WebsiteGoogleScholarHomepage](/img/swirl/WebsiteGoogleScholarHomepage.png)

先说说 Google Scholar 的屏蔽问题。对于能够访问（无论通过什么方式达成）的用户来说，依然是会遭遇 Google 的封锁的。

一方面是 Google 对于学术搜索有着很严格的管控，操作频繁很容易就被判定为滥用或程序爬取之类的恶意行动，而被封锁 IP；另一方面，也是出于防止滥用，预防性地对诸多空间服务商的 IP 进行了封锁，而连带着扎根于此之上的 VPN 或 Shadowsocks 等服务也被牵连，导致无法正常访问 Scholar。

遇上前者，如果是人为的偶然操作导致，多半会在短期内就解封，而如果是被判定恶意或 IP 早就被屏蔽，那等待也没办法解决这种问题。最佳的方式还是切换 IP。

Chrome 浏览器搭配代理工具 [Proxy SwitchySharp](https://chrome.google.com/webstore/detail/proxy-switchysharp/dpplabbmogkhghncfbfdeeokoefdjegm?hl=en) 来绕开 Google 的封锁。

操作方式蛮简单的:

1. 下载和安装改扩展程序;
2. 为 SwitchySharp 导入规则，规则导入后，Proxy Profiles 和 Switch Rules 都会导入相应设定；
3. 在 Chrome 扩展程序栏，点击 SwitchySharp 的图标，切换为对应规则，比如，使用 Shadowsocks 作为服务，则切换到 SS 的选项。
4. 而后，点击 Auto Switch Mode 作为首选方式。正常情况下，刷新 Google 学术首页就可以正常使用了。

![SwitchySharp](/img/swirl/ChromeExtensionProxySwitchySharpSetting.png)

当然，需要注意一点，在不访问 Scholar 的时候，如果继续使用 SwitchySharp，记得将 Mode 从 Auto Switch Mode 切换为 System Mode，让系统（其实就是自己用的 SS 服务或其它）接管代理规则设定，正常的网络访问会受影响。