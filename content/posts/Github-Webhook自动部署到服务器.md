+++
title = "Github-Webhook自动部署到服务器"
date = 2023-04-15
draft = false
[taxonomies]
tags=["github","webhook","rust","html","deploy"]
[extra]
toc=true
+++

### 起因
Github Page 虽然好用，但是国内访问速度总是缓慢。

于是打算和github action配合把构建好的静态页面部署到服务器nginx下。

### 配置

#### 配置github
github 项目配置页面设置webhook地址。

可以选择在只有push事件的时候触发webhook


#### 配置服务器

我采用这个rust写web服务 [source](https://github.com/cyejing/github-webhook-server)

通过配置文件，可以暴露出web接口。

当webhook触发，会对比触发的项目和配置的项目，以及配置的事件类型。

当匹配成功会在`repo_directory`目录执行git更新，并且在`working_directory`执行`command`。

#### 配置nginx

nginx配置映射`repo_directory`目录的静态文件。

### 总结

整个流程如下：

- 当提交代码时会触发github-aciton, 并且构建出静态页面推送到`gh-pages`分支。

- 当`gh-pages`分支受到推送事件会触发webhook到服务器。

- 服务器执行git更新，静态页面服务nginx得到更新
