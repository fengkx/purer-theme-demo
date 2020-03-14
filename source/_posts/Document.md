---
title: 文档
date: 2020-03-14 11:54:24
tags: [Doc]
permalink: document-zh
---

# 基本概念

[Hexo](https://hexo.io) 是一个静态博客框架。假设你已经按照[官方文档](https://hexo.io/docs/)安装好了 Hexo。

此时，站点目录下将会有一个`_config.yml`文件称为**站点配置文件**。同时还有一个`themes`目录用于存放主题。`themes`目录下的每一个文件夹都是一个 Hexo 主题。每个主题文件夹内也有一个`_config.yml`文件称为**主题配置文件**。

Hexo 通过指定站点配置文件的`theme`属性为`themes`下的文件夹名称指定主题。

站点配置文件和主题配置文件都使用 [YAML(YAML Ain't Markup Language)](https://yaml.org/) 格式。该格式有几个特别需要注意的点
1. 格式主要形式是键值对，通过`: `分隔键值，**冒号后面需要有空格分隔**
1. 通过**缩进**来指示配置层级
1. 字符串值可以有可选的引号
1. `-` 开头的为列表项
1. `#` 开头的行为注释

例如
```yaml
site:
  favicon: /images/favicon.ico
  site_verification:
    google: 'ABC'
#   baidu:
```
下面我们通过用对象取值的方式表示相关的属性。`ABC`这一属性值可表示为`site.site_verfication.google`

# 安装

安装需要做三件事
1. 将主题下载到`themes`文件夹下并做好命名
1. 改写主题配置文件并安装相应的依赖
1. 在站点配置文件的`theme`属性指定主题

如果你使用 Git 管理你的博客源文件，推荐你使用`Git submodule`来安装主题。
```sh
git submodule add https://github.com/fengkx/hexo-theme-purer themes/purer
```
运行完成后将会在`themes/purer`下克隆`purer`主题并生成一个`.gitmodules`文件。
![gitmodules.png](https://i.loli.net/2020/03/14/ihvBKmdjrLxHwVO.png)
你应该将这个文件 checkout 到 Git 中。

为了避免冲突问题，`purer`主题将`_config.yml`加入到了`.gitignore`中。为了避免更新主题时出现冲突，建议不要直接改动主题内的文件。
我们可以将真正的主题配置文件放在站点目录下
```sh
cp themes/purer/_config.example.yml _config.theme.yml
```
然后在构建过程之前将该文件复制到主题的根目录中。我们可以通过`npm script`来实现。改写站点目录下的`package.json`
```json
"scripts": {
    "theme": "cat ./_config.theme.yml > ./themes/purer/_config.yml ",
    "build": "npm run theme && hexo generate",
    "clean": "hexo clean",
    "deploy": "npm run theme && hexo deploy",
    "server": "npm run theme && hexo server"
  }
```
这样，我们通过`npm run build`等执行`npm scripts`时，文件将会自动更新至主题目录中。该方法也可以通过带构建的静态托管网站(例如 Netlify)或者CI实现。


接下来我们只需要在站点配置文件中指定`theme`为`purer`

尝试`npm run server`你应该能看到类似这样的外观。

![new_init.png](https://i.loli.net/2020/03/14/4lmxLaybI5VSjGO.png)


## 更新主题
在主题的根目录下`git pull origin master`即可。

## 依赖安装
卸载用不到的依赖
```sh
npm uninstall hexo-renderer-stylus
npm uninstall hexo-renderer-marked
```
使用 markdown-it
```sh
npm i -S hexo-renderer-markdown-it
```
支持从`post_assert_folder` 用 markdown 引入图片。
```sh
npm i -S hexo-asset-image
```
支持 emjoi 和 mathjax
```sh
npm i -S markdonw-it-emjoi
npm i -S markdown-it-mathjax
```


# 页面创建
默认情况下 `[categories, tags,  repository, links, about]`页面都不会被创建。你需要在`source`目录下创建对应名称的目录并创建`index.md`。并在`front-matter`中指定`layout`
目录结构如下
```plain
.
├── about
│   └── index.md
├── categories
│   └── index.md
├── links
│   └── index.md
├── repository
│   └── index.md
└── tags
    └── index.md
```

例如`repository`页面的`index.md`内容为
```md
---
title: Repositories
layout: repository
comments: false
sidebar: none
---
```

你可以通过`front-matter`控制是否显示右侧的 sidebar

类似的你也可以通过这种方式，自定义`404`页面。样例目录结构：
```plain
404
├── cover.jpg
├── happy-time.mp3
├── index.md
└── lrc.txt
```

# 友链
友链与主题分离，数据存放在[数据文件夹](https://hexo.io/docs/data-files)的`links.yml`中
例子：
```yml
# links
fengkx0:
  link: https://www.fengkx.top
  avatar: https://www.fengkx.top/images/avatar.jpg
  desc: 测试测试

fengkx1:
  link: https://www.fengkx.top
  avatar: https://www.fengkx.top/images/avatar.jpg
  desc: 测试测试

```
效果可以参考[Demo](https://www.fengkx.top/links/)




# 本地搜索

首先安装`hexo-generator-json-content`
```sh
npm i -S hexo-generator-json-content
```

# 主题配置

## menu
是否显示左侧 header 中的菜单，如果不需要可以注释掉。

## menu_icons
`enable`左侧菜单是否显示图标，如不需要可以设置成`false`
其他配置项的值为`iconfont`的`class`。一般不需要更改，如果你需要新增图标可以提 [Issue](https://github.com/fengkx/hexo-theme-purer/issues) 或者更改图标可以更改主题的`src/css/iconfont.css`。图标来自阿里的 [iconfont.cn](https://iconfont.cn)。

## site
### favicon
站点 favicon，相对`source`或`theme`
例如`/images.fvicon.ico`相当于在`source/images/favicon.ico`中找。

### site_verfication
Google 或 Baidu 提供的 HTML meta 验证。形如
```html
    <meta name="google-site-verification" content="your verification string">
```
将 content 里的内容粘贴到对应的属性中。

## pagination
prev， next 是否总是显示，默认为 true。仅当多于一页时有效。

## comment

### type
选择启用哪一种评论系统。留空则不启用。
你可以通过`front-matter`的`cooment: boolean`控制具体一篇 post 是否开启评论。默认开启。

## github
### username
GitHub username

## postCount
### enable
是否启用。需要安装相关的插件
```sh
npm i --save hexo-wordcount
```
### wordocunt
是否显示文章字数统计
### min2read
是否显示阅读时长预计

## toc
TOC 全局开关
你可以通过`front-matter`的`toc: boolean`控制是否具体一篇 post 是否开启 toc。默认开启。

## fancybox
是否开启基于[LightGallery](https://sachinchoolur.github.io/lightgallery.js/)图片 FancyBox 效果。默认开启。

## license
Copyright 显示的 HTML 片段。注释掉则不显示Copyright

## footer
### custom
自定义 Footer 的 HTML 片段。注释掉则不显示。

具体例子可参考 [Demo](https://purer.netlify.com)
Demo 源文件 [Source](https://github.com/fengkx/purer-theme-demo)

## profile
### enable
是否显示 profile
效果可以参考 [Demo](https://purer.netlify.com)
Demo 源文件 [Source](https://github.com/fengkx/purer-theme-demo)

### social
#### links
Footer 显示的社交联系图标和链接。如需要添加图标可以提 [Issue](https://github.com/fengkx/hexo-theme-purer/issues)

### links
About 页面侧栏显示的链接
### labels
About 页面侧栏显示的 Labels
### skills
About 页面侧栏显示的 Skills
### works
About 页面侧栏显示的个人项目

## widgets
侧栏显示的 widget， 注释则关闭。
### show_count
侧栏的 tag， archive，category widget 中是否显示文章数量

## cdn
用到的 CDN 地址，如果你不知道是什么就不要改动。