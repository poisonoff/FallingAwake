---
title: "Hallooo copy"
date: 2023-01-13T11:42:06+01:00
draft: false
Tag: TEST
---

**hugo速度超快**，目前为止这个blog中大概有十几篇文章，每次生成全站的速度都不超过10ms，也能做到实时预览，真是爽快！

## hugo 第一步首先，找到安装包，点[这个链接](https://github.com/gohugoio/hugo/releases)找到你需要的版本，只有 **6MB** 大，真是感动的要哭了……当然作为mac用户你也可以选择输入 `brew install hugo` 来快速安装。什么，你不知道brew，哪你不妨先搜索一下brew吧。找到对应的文件路径，输入`hugo new site yoursitename `就创建好了文件夹，里面包括了以下的文件结构：content 用来放文本和页面内容的地方archetypes 用来放 `.md` 文件的模板data 用来放特殊定义的数据（json yaml之类的）layouts 用来放页面模板，也是我们后面会魔改的地方static 用来放静态文件themes 主题public 用来存放渲染出来的静态文件此时输入`hugo server  `会在本地启动一个服务器，可以访问 http://localhost:1313 看一眼。当然这个链接里面完全是空的，什么都没有，毕竟文件夹里全部都是空的，没有内容也没有主题。

## 添加主题添加主题其实很方便，首先到hugo的[主题官网](https://themes.gohugo.io/)看一下，挑选一个心意的主题，或者到github上搜一个star数高的也行。我为了魔改方便，选了一个非常非常轻量的主题 [hugo-paper](https://github.com/nanxiaobei/hugo-paper)，作为blog的主题。`cd yoursitename/ git submodule add https://github.com/nanxiaobei/hugo-paper.git themes/hugo-paper; `然后修改根目录下的 `config.toml` ，这是blog中的基本设置，其中有几个比较重要：`baseURL = "" # 主机名称，后面会提到 title = "" # blog的标题 theme = "hugo-paper" # 使用的主题，对应 /themes/你的主题名称 `其他还有很多有用的参数，想要了解可以[看这里](https://gohugo.io/getting-started/configuration/#toml-configuration)。有了主题之后，就差添加第一篇文章了！

## 第一篇文章直接输入`hugo new post/fisrt-post.md `就会在 `content/post` 下新建一个md文件，只需要编辑里面的内容，比如加一句 hello world ，马上就能在浏览器中看到渲染出来的文章了，速度非常快。如果想要添加其他的文章，只需要继续新增文章即可。至于怎么新增其他类型的文章，或者应用文章模板，我们后续再说。

## github page 够用吗？之前用hexo的时候一直在使用github page，算是比较稳定的免费服务，但是体验上比较差的地方在于github page 的页面渲染需要在本地完成然后发 commit 上去。然而hexo的编译经常出问题，三天两头报错，因此我看了其他的方案。首先是用 Travis-CI 的方式，通过自动化追踪的方式来部署页面，大概需要这么做：生成github token用 travis ci的命令行工具加密token，然后把这个配置到配置文件中。用 travis ci 向 page 页面的 gh-pages 提交渲染后的静态文件能配置倒是能配置，就是太麻烦了。

## Netlify 闪亮登场！通过朋友的介绍，我找到了 [netlify](https://www.netlify.com/) 这个服务。它对自己的定义是：An all-in-one workflow that combines global deployment, continuous integration, and automatic HTTPS. And that’s just the beginning. 一个包含全球部署，持续化集成以及自动 HTTPS 的全能工作流。使用它很简单，包括三步：连接 git repo：支持 github gitlab 以及 bitbucket。添加编译设定：比如设定分支，编译命令，以及输出的文件。等待自动部署好。对于免费用户来说，流量和部署速度都没有打折，还是非常贴心的。

## Netlify 使用指南想要和 hugo 联动也是非常的简单。![img](https://blog-1254831322.cos.ap-beijing.myqcloud.com/blog/2019-05-13-040909.jpg)首先连接 github 账号，在 repo 列表中找到对应的 repo。![img](https://blog-1254831322.cos.ap-beijing.myqcloud.com/blog/2019-05-13-040910.jpg)netlify 会读取出来这个 repo 的类型，如果是 hugo 就会自动填写上 bulid command 和 publich directory。我们不用管这个，直接点最下面的 *Deploy site*。![img](https://blog-1254831322.cos.ap-beijing.myqcloud.com/blog/2019-05-13-040913.jpg)稍等一下，页面就会自动部署完，你可以在 Deploys 的页面中看到每一次部署的结果，如果后面有一个 *Failed* 标志，说明部署失败了。如果一切正常，那么你之后的每一次 git 提交，都会触发 netlify 重新编译部署一次页面，很快blog 就展示全新的内容了，这种感觉非常爽！

## 设置自定义域名和 HTTPS在 netlify 中当然也可以设置自定义域名，点击 *Settings*，找到 *Domain management*，就可以添加自定义域名了。netlify 本身也会给你生成一个默认的二级域名，也是可以用的。添加域名的过程其实就是在 DNS 服务中增加一个 A 记录的转发，我使用的是 DNSPOD，在设置中直接新增一条，将根域名 wocai.de 转发到 netlify 提供的 ip 地址上就好了。netlify 还提供了免费的 https 证书服务，证书由 Let’s Encrypt 提供，一键点击即可，非常之方便。

## 后记在告别了 wordpress 和 hexo 之后，我最终选择了非常轻量的静态生成器 hugo 和优秀的部署服务 netlify，经过合理的组合，终于不用再每次编译提交了。但超级轻量的 hugo 带来了另外一个问题，太简单了，什么都没有，这意味着所有的功能都需要自己研究然后改造模板。这部分内容留着下次说罢。
