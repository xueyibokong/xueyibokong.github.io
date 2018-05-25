---
title: Github Pages + hexo 建博客
date: 2018-05-25 10:30:12
tags: 
	- blog
	- Github Pages
	- hexo
---

# GitHub Pages + Hexo To My Blog
## xueyibokong.github.io
我自己的github仓库

## Hexo [remote是Hexo For xueyibokong.github.io] [Hexo 文档](https://hexo.io/zh-cn/docs)
blog建站工具
通过配置 `_config.yml` 的 `Deployment` 来关联 `xueyibokong.github.io`

## 目录分析
#### `_config.yml`
全局配置文件，网站的很多信息都在这里配置，诸如网站名称，副标题，描述，作者，语言，主题，部署等等参数。这个文件下面会做较为详细的介绍。
#### scaffolds
scaffolds是“脚手架”的意思，当你新建一篇文章（hexo new 'title'）的时候，hexo是根据这个目录下的文件进行构建的。基本不用关心。
#### source
这个目录很重要，新建的文章都是在保存在这个目录下的._posts 。需要新建的博文都放在 _posts 目录下。
_posts 目录下是一个个 markdown 文件。你应该可以看到一个 hello-world.md 的文件，文章就在这个文件中编辑。
_posts 目录下的md文件，会被编译成html文件，放到 public 文件夹下。
#### themes
网站主题目录，hexo有非常好的主题拓展，支持的主题也很丰富。该目录下，每一个子目录就是一个主题。
## 管理
## 文章多点发布 & 同步
### 发布
#### 创建文章
文章默认创建在source/_post目录下，
```
$ hexo new [layout] "My New Post"
```
创建文章的`.md`文件会在头部生成文章基本信息
```
---
title: test hexo
date: 2018-05-25 10:30:12
tags: 
	- q
	- w
type:	
---
```
>`title` 文章标题
>`date`  时间
>`tags`  文章标签
>`type`  文章类型
#### Run server [查看效果]
```
$ hexo server
or
$ hexo s
```
#### 构建静态文件
初始化静态文件，也就是将source/_post目录下`.md`的文章构建到public目录下
```
$ hexo generate 
or
$ hexo g
or 
$ hexo server -p 端口号
```
#### 推送到 username.github.io
将public目录下的静态web推送到`username.github.io`仓库下
```
$ hexo deploy
```
### 换点发布
#### 场景 
换一台电脑发布文章。
#### 方案
1、将hexo项目保存到 username.github.io项目的hexo分支上。
2、将hexo分支设为default分支，便于clone和提交。
## 主题应用
在 Hexo 中有两份主要的配置文件，其名称都是 _config.yml。 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置有主题作者提供，主要用于配置主题相关的选项。 
为了描述方便，在以下说明中，将前者称为 站点配置文件， 后者称为 主题配置文件。
### 安装主题
```
git clone [主题仓库地址] themes/[主题名]
```
### 启动主题
修改根目录下的`_config.yml`的`theme`配置项
```
theme : [主题名]
```
### 本地验证
```
$ hexo s
```
### 推送到线上生效
```
$ hexo deploy
```
