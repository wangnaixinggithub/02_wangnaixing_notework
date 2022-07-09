# Hexo

# 1、Download

- 下载hexo搭建hexo工程

```shell
# 安装heox-cli
npm install -g hexo-cli

# 初始化hexo工程 名称为 website-hexo
hexo init website-hexo

# 进入website-hexo
cd website-hexo

# 安装hexo所需npm依赖
npm install
```

- 下载hexo主题，比如`hexo-theme-matery`，解压放到hexo工程的theme文件夹下

```properties
下载网址=https://github.com/blinkfox/hexo-theme-matery
保存解压文件的容器文件夹名称=hexo-theme-matery
```

# 2、hexo/_config.yml

- hexo工程下有_config.yml，此为hexo的全局配置。

```yaml
search:
  path: search.xml
  field: post
  
# 代码高亮  
highlight:
  enable: false
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs:
  enable: true
  preprocess: true
  line_number: true
  tab_replace: '' 
  
#部署配置 
deploy:
  type: git
  repo: https://github.com/wangnaixinggithub/wangnaixinggithub.git
  branch: main  
```

# 3、hexo-theme-matery/_config.yml

- 使用hexo命令创建这四个导航项

```shell
hexo new page "categories"
hexo new page "tags"
hexo new page "about"
hexo new page 404
```

- 会新增四个文件夹，都是以他们名称命名的，每一个文件夹下又有index.md。依次加入下面内容。

```markdown
---
title: categories
date: 2021-09-06 10:19:56
type: "categories"
layout: "categories"
---

---
title: tags
date: 2021-09-06 10:25:04
type: "tags"
layout: "tags"
---

---
title: about
date: 2021-09-06 10:28:56
type: "about"
layout: "about"
---

---
title: 404
date: 2021-09-06 10:32:48
type: "404"
layout: "404"
description: "Oops～，我崩溃了！找不到你想要的页面 :("
---
```

# 4、Wirte Md

- 确保Md文件放到_post目录下。

- 每一个文件加上这样的信息头

```markdown
---
title: 001-Spring-HelloWorld
date: 2022-05-02 17:49:11
permalink: /pages/c68801/
categories:
- 01_Spring配置
- 01_XML配置
---
```

# 5、GitHubPage

- 安装推送github仓库插件

```shell
npm install hexo-deployer-git --save
```



