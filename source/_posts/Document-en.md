---
title: Document
date: 2020-03-14 22:59:19
tags: [Doc]
permalink: document-en
---

# Basic concept

[Hexo](https://hexo.io) is a static Blog Framework. Assuming you have installed Hexo under the guidance of [offical Document](https://hexo.io/docs/).

You can find a file named`_config.yml`. Let's call it **site config file**. At the same time, You should find a folder named `themes`, which is used to store Hexo theme. Every folder in `themes` is a Hexo theme. Every theme should also have a `_config.yml`. Let's call it **theme config file**.

Hexo theme is set through the `theme` property in site config file. It should be the name of a folder in `themes` folder.

Both the site config file and theme config file are using [YAML(YAML Ain't Markup Language)](https://yaml.org/). There are some points we should pay attention to.
1. The format is mainly about key-value pair，It is divided by a`: `. **There should be at least one space between colon and value**
1. The hierarchical relationship is indicated by **indent**
1. We can optionally add quotation marks to a string value
1. A line beginning with `-` is a list item 
1. A line beginning with `#` is a comment

For example
```yaml
site:
  favicon: /images/favicon.ico
  site_verification:
    google: 'ABC'
#   baidu:
```
We can say the propery with value of `ABC` as `site.site_verfication.google`

# Installation

There are three things to do
1. Download the theme to `themes` folder and name it correctly
1. rewrite the theme config file and install the dependencies
1. set the `theme` property in site config file

If you use Git to maintain the source file of your blog，`Git submodule` is recommended to install the theme。
```sh
git submodule add https://github.com/fengkx/hexo-theme-purer themes/purer
```
After running the command above, `purer` theme is cloned under `themes/purer`. And a `.gitmodules` is generated.
![gitmodules.png](https://i.loli.net/2020/03/14/ihvBKmdjrLxHwVO.png)
You should check out this file in Git too.

To avoid conflict, `purer` theme add `_config.yml` into `.gitignore`. For the same reason, I suggest you never change the file inside the theme directly.
We can put the real theme config file into the site root directory.
```sh
cp themes/purer/_config.example.yml _config.theme.yml
```
Then we can copy this file into the theme's root directory when building. We can do this by `npm script`. Rewrite the `scripts` part of the `package.json` in the site root directory.
```json
{
  "scripts": {
      "theme": "cat ./_config.theme.yml > ./themes/purer/_config.yml ",
      "build": "npm run theme && hexo generate",
      "clean": "hexo clean",
      "deploy": "npm run theme && hexo deploy",
      "server": "npm run theme && hexo server"
    }
}
```
After that, every time we run a `npm scripts` such as `npm run build`. The theme config file get updated. It can also achive by a static site host platform with building process support(such as Netlify) or a CI.

Then we just set the `theme` property to `purer` in the site config file.

Try `npm run server`, You should get a site like this.

![new_init.png](https://i.loli.net/2020/03/14/4lmxLaybI5VSjGO.png)


## Updating theme
Just run `git pull origin master` in the theme root directory.

## Install dependencies
Uninstall the dependencies we don't use.
```sh
npm uninstall hexo-renderer-stylus
npm uninstall hexo-renderer-marked
```
Use markdown-it instead of marked
```sh
npm i -S hexo-renderer-markdown-it
```
`hexo-renderer-markdown-it` do not generate the anchor of `h1`, So we need to add some configurations in site config file.
```yml
markdown:
  html: true
  xhtmlOut: true
  breaks: true
  langPrefix:
  linkify: true
  typographer:
  quotes: “”‘’
  anchors:
    level: 1
    permalink: false
    separator: '-'
```
For add image using markdown from `post_assert_folder`
```sh
npm i -S hexo-asset-image
```
For emjoi and mathematical formula
```sh
npm i -S markdown-it-emjoi
npm i -S @iktakahiro/markdown-it-katex
```
We need to load some plugins. End up we got thing like this.
```yml
markdown:
  html: true
  xhtmlOut: true
  breaks: true
  langPrefix:
  linkify: true
  typographer:
  quotes: “”‘’
  anchors:
    level: 1
    permalink: false
    separator: '-'
  plugins:
    - '@iktakahiro/markdown-it-katex'
    - markdown-it-emjoi
```


# Page creation
The page of  `[categories, tags,  repository, links, about]` does not create by default. You need to create a folder inside the `source` directory with a corresponding name and create an `index.md` in it. Then set `layout` in `front-matter`.
Directory structure like this
```plain
source
├── about
│   └── index.md
├── categories
│   └── index.md
├── links
│   └── index.md
├── repository
│   └── index.md
└── tags
    └── index.md
```

For example, the `index.md` under `repository` folder has content like this.
```md
---
title: Repositories
layout: repository
comments: false
sidebar: none
---
```

你可以通过`front-matter`控制是否显示右侧的 sidebar

You can also customize `404` page like this. Example directory structure
```plain
404
├── cover.jpg
├── happy-time.mp3
├── index.md
└── lrc.txt
```

# Links
Links data is sperated with theme，It is stored in 的`links.yml` under [data folder](https://hexo.io/docs/data-files).
For example:
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
We can see the result in [Demo](https://www.fengkx.top/links/)

# Local search

`hexo-generator-json-content` is needed.
```sh
npm i -S hexo-generator-json-content
```

# Theme configuration

## menu
Whether show menu in the header on the left-hand side, Comment out if you don't need it.

## menu_icons
`enable` Wether show icon on the left-hand side menu，set to `false` if you don't need it.
others part is the `class` of `iconfont`. Generally, we don't need to change it. If you want to add new icons, Welcome to post an [Issue](https://github.com/fengkx/hexo-theme-purer/issues) or change the `src/css/iconfont.css` file. Icon is provided by Alibaba's [iconfont.cn](https://iconfont.cn).

## site
### favicon
site favicon path，relateive to `source` or `source` under the theme root directory.
For example `/images.fvicon.ico` is treated as`source/images/favicon.ico`.

### site_verification
HTML meta verification provided by Google or Baidu such as
```html
    <meta name="google-site-verification" content="your verification string">
```
Paset the string in content into the corresponding property.

### google_analytics
Set a track ID provided by Google Analytics. Disabled when empty.

## pagination
prev， next whether always show the button，`true` by default. Only work when there is more than one page.

## comment

### type
Choose which comment system to use. Disabled by leave it blanks
You can control whether enable comment on a certain post by setting `comment: boolean` in `front-matter`. The default value is true.

## github
### username
GitHub username

## postCount
### enable
Whether enable. Extra dependency is needed.
```sh
npm i --save hexo-wordcount
```
### wordocunt
Whether show word count
### min2read
Whether show minutes to read.

## toc
TOC global switch
You can controll whether enable toc on a certain post by setting `toc: boolean` in `front-matter`. The default value is true.

## fancybox
Whether enable Fancybox image effects based on[LightGallery](https://sachinchoolur.github.io/lightgallery.js/). Enabled by default.

## license
Copyright HTML fragment. Comment out if you don't want to show Copyright.

## footer
### custom
Customize Footer HTML fragment. Comment out if you don't want to show extra footer.

A [Live Demo](https://purer.netlify.com)
Demo source file [Source](https://github.com/fengkx/purer-theme-demo)

## profile
### enable
Whether display profile
You can see the effect on [Demo](https://purer.netlify.com)
Demo 源文件 [Source](https://github.com/fengkx/purer-theme-demo)

### social
#### links
links and icons in footer。If you want to add new icon, Welcome to post an [Issue](https://github.com/fengkx/hexo-theme-purer/issues).

### links
Links shown in the sidebar of about page
### labels
Labels shown in the sidebar of about page
### skills
Skills shown in the sidebar of about page
### works
Wroks shown in the sidebar of about page

## widgets
widget shown in the sidebar. Comment out to disable
### show_count
Whether show post count at tag， archive，category widget in the sidebar

## cdn
CDN uri，don't change it if you don't know what it is.
