---
title: Windows上Hexo环境搭建
hidden: false
katex: false
date: 2022-07-07 15:02:15
id: Hexo-on-Windows
sticky:
categories:
- 计算机相关
tags:
- Hexo
- Windows
---

最近又开始折腾 Windows 上的编程/工作环境，为了能够记录下所遭遇的各种问题，故先配置了 Hexo 来进行记录。

<!-- more -->

众所周知，Hexo 需要两个工作环境：

- Git
- Node.js

所以我们只需要进行相关的配置即可。

对于 Git 的配置，只需要在官网下载并安装即可，过程中没有出现什么问题。

重点是 Node.js 的安装和 npm 的使用。到官网下载 Node.js Windows 版本的 LTS（长期维护版）,然后安装即可。此时在 shell 中输入 

```she
npm --version
```

如果输出版本号则表示安装成功。

之后安装 Hexo

```shel
npm install -g hexo-cli
```

我出现了错误

```shell
npm ERR! code ERR_INVALID_URL
npm ERR! Invalid URL
```

后发现是代理出现问题，首先清除 npm 的代理设置

```she
npm config delete proxy
npm config delete https-proxy
```

然后查看是否清除成功

```shell
npm config get proxy
```

如果输出

```shell
null
```

即代表清除成功，之后再设置代理就能解决问题

```shell
npm config set  proxy http://yourproxy
npm config set  https-proxy http://yourproxy
```

这样就能快乐地安装并运行 Hexo 写 Blog 啦~