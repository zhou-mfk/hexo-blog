---
title: github+hexo安装及配置
date: 2019-01-06 18:55:30
categories: 记录 
tags: ["github", "hexo", "git"]

---

# github + hexo

## 安装篇

### 安装nodejs

```shell
sudo apt install nodejs
sudo apt install npm
# 调整npm的仓库地址 
npm config set registry http://registry.npm.taobao.org

```

### 安装hexo

```shell
sudo npm install hexo-cli -g
sudo npm install hexo -g
npm install --save hexo-deployer-git
```

### hexo 常用命令

```shell
hexo init # 初始化网站
# 新建文章
hexo new "文章名" 
hexo n "文章名" # 此为简写
# 生成静态文件
hexo generate
hexo g
# 启动本地服务器
hexo server
hexo s
-p 指定端口，默认为4000
-i 指定服务器ip地址
-s 静态模式，仅提供public文件夹中的文件并禁用文件监视
# 部署
hexo deploy
hexo d
# 通常与 generate配合使用
hexo g -d

```

## 配置篇

### 初始化 hexo

```shell
# 创建hexo目录
mkdir hexo
cd hexo
# 初始化
hexo init # 此命令只能在空目录下运行

```

### hexo 使用NexT主题

```shell
# 下载主题
cd hexo/themes
git clone https://github.com/theme-next/hexo-theme-next.git

```

### hexo 配置

#### 网站配置

编译网站配置 hexo/_config.yml 

```yml
# Site
title: 点滴记录
subtitle: 记录一些东西
description: Code and Record
keywords: Myself
author: Zhou Li Shan
language: zh-CN # 主题Next中的language使用的zh-CN
timezone: Asia/Shanghai

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://zhou-mfk.github.io
root: /
permalink: :year/:month/:day/:title/ # 可以自己修改
permalink_defaults:
# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:
# 主题配置
## Themes: https://hexo.io/themes/
# theme: landscape
theme: hexo-theme-next

# 部署到github的配置
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: <you github>
  branch: master
```

#### 主题配置

编辑主题配置文件 hexo/themes/hexo-theme-next/_config.yml

```yml
# 修改footer
footer:
  since: 2019
  icon:
    name: user
    animated: false
    color: "#808080"
  copyright: # 
  powered:
    enable: false
    version: false
  theme:
    enable: false
    version: false

```

主题有四个schemes

```yml
# Scheme Settings
# scheme: Muse
# scheme: Mist
scheme: Pisces
#scheme: Gemini

```

配置社交信息

```yml
social:
  GitHub: https://<github> || github
  E-Mail: mailto:<email> || zhoulishan

```

还有其他主题优化。可以在网上找到这里就不搬了。





