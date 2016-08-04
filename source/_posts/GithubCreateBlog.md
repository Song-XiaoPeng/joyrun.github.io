---
layout:     post
title:      "如何利用github创建博客"
date:       2016-07-21 14:22:52
author:     "Wiki"
tags:
    - 其它
---



### 分析原理
> 我们博客地址：https://joyrun.github.io
> 我们博客的托管地址：https://github.com/joyrun/joyrun.github.io

可以从托管地址看到，我们的博客其实就是一个静态的网站。其实github给每个用户和组织都提供了一个静态网页托管，地址就是[用户名/组织名].github.io。比如 https://joyrun.github.io
### 利用Hexo创建静态博客
网上有很多的开源框架可以用来创建静态博客，其中Hexo就是非常有名气的一个，被很多人使用，也有大量的主题可以选择。 https://hexo.io
#### 安装hexo
安装前需要先安装npm包管理器
> npm install hexo-cli -g
#### 创建初始化项目
创建初始化项目有两种方式可以选择，第一种就是直接使用命令创建默认的项目，样式有点丑；第二种就是在github上面clone一个带有主题的项目 https://hexo.io/themes/。
```
hexo init <folder>
或
git clone https://github.com/Kaijun/hexo-theme-huxblog.git

cd <folder>
npm install
// 如果出现问题，可以试试 npm install hexo --save
```
就可以看到一个目录结构如下的项目
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
#### 修改配置_config.xml
可以把博客名称改成自己的，还可以配置一些和博客相关的东西，还要把部署的地址改成我们github对象的仓库地址。
#### 编译并发布
```
//清理缓存
hexo clean
// 生成静态的网站，可以在
hexo generate
// 发布
hexo deploy
// 在本地浏览
hexo server
```
