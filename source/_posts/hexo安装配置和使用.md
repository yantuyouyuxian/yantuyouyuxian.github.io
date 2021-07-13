---
title: hexo配置和基础命令
date: 2021-06-20 19:09:38
categories: 
- hexo
- config&use
tags:
- hexo
cover: false
myPath: post/hexo-config
---

# hexo的安装

hexo的安装配置教程众多，百度即可

需要注意的几点：

- 想要部署到github需要先安装部署插件

- 部署配置

  ```yaml
  deploy:
    type: git
    repo: git@github.com:yantuyouyuxian/yantuyouyuxian.github.io.git
    branch: main
  ```

- github和gitee同时部署

  ```yaml
  deploy:
    - type: git
      repo: git@github.com:yantuyouyuxian/yantuyouyuxian.github.io.git
      branch: main
    - type: git
      repo: https://gitee.com/yantuyouyuxian/yantuyouyuxian.git
      branch: main
  ```
  
- 如何在将博客部署到远端的同时，将本地资源路径使用github进行备份方便迁移

  详细参考链接：[hexo博客的备份和迁移_小皮子摘星星的博客-CSDN博客](https://blog.csdn.net/qq_37391214/article/details/100186909)

# hexo的常用命令

安装：

```bash
# 全局安装hexo框架
npm install -g hexo-cli
# 安装hexodeploy插件
npm install --save hexo-deployer-gi
# 如果使用butterfly主题，还需要安装pug和stylus的渲染器
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

使用：

```bash
# 一篇新文章
hexo new "文章名称"
# 一个新页面
hexo new page "页面名称"
# 清除
hexo clean
# 生成
hexo generate
# 本地启动
hexo server 
# 本地启动并指定端口号
hexo server -p 5000
# 部署到远端
hexo deploy
# 多个命令的组合
# (注意：两个&会将多个命令按顺序执行)
hexo clean && hexo generate && hexo deploy && hexo server
```

# 域名绑定后每次deploy后404问题

推送到github后每次自定义的域名都会被置空，解决办法：

- 在博客的post目录下新建CNAME文件
- 在文件中输入自定义域名即可

