---
title: 使用TravisCI给hexo的部署做持续集成
categories:
  - hexo
tags:
  - hexo
  - TravisCI
cover: false
date: 2021-06-29 19:57:37
---

# 为什么要使用持续集成？

首先来回忆下使用hexo发布一篇博客的基本步骤吧：

1. ```hexo new "xxx"```
2. 写博客
3. ```hexo c```
4. ```hexo g```
5. ```hexo d```

以上是hexo写一篇博客的几个基本步骤，值得一提的是hexo g和hexo d两个操作，generate是hexo将我们写的md文件转换成网页静态资源文件的过程，执行这个命令后，你的博客根目录下的public文件夹中的文件就是deploy到git上的文件夹，你可以比对确认。

此外，如果你像我一样，还把hexo的资源文件（不是public目录下生成部署使用的网页资源）同步推送到git上进行备份，那么还需要使用额外的git命令：add，commit，push。

这样一看是不是就有些麻烦了呢？而持续集成就可以十分方便的简化我们维护博客的过程，配置完成后，你在写完一篇博客后只需要在blog根目录执行git push操作，让CI工具帮你处理hexo的一系列生成过程即可。

# 怎么使用？

1. 首先注册Travis-CI账户，可以直接使用GitHub账户登录；

2. 注册完成后到GitHub上生成自己的token，之所以使用token是因为CI环境没有我们的SSH密钥，但hexo的deploy会操作我们的仓库；

3. 有了token后，复制，到Travis-CI点击你的博客仓库，右上角点击进入设置，添加一个环境变量（环境变量名无所谓，方便识别就行，如：GITHUB_TOKEN），值就是生成的token，如图所示：

   ![img](travis.png)

4. 在博客根目录添加```.travis.yaml```文件，本人配置内容如下：

   ```yaml
   language: node_js #指定使用的语言
   
   node_js: stable  #采用nodejs的最新版本
   
   # 指定缓存模块，可选。缓存可加快编译速度。
   cache:
     directories:
       - node_modules
   
   before_install:
       - export TZ='Asia/Shanghai' # 更改时区    
   
   # Start: Build Lifecycle
   install:
     - npm install
   
   # 执行清缓存，生成网页操作
   script:
     - hexo clean
     - hexo generate
   
   # 指定博客分支
   branches:
     only:
       - hexo #触发持续集成的分支
   
   # 指定博客的仓库地址
   env:
    global:
       - GH_REF: https://${GITHUB_TOKEN}@github.com/yantuyouyuxian/yantuyouyuxian.git
   # 设置git提交名，邮箱；替换真实token到_config.yml文件，最后depoy部署
   after_script:
     - cd ./public
     - git init
     - git config user.name "xxxx"
     - git config user.email "xxxx@qq.com"
     - git add .
     - git commit -m "TravisCI 自动部署"
     - git push --force --quiet "${GH_REF}" master:main
   
   ```

# 踩的坑

{% note info simple %}

整个过程唯一要注意的一点就是将CI生成的public文件夹push到git的路径设置问题，也就是```.travis.yaml```中after_script部分。

- 首先进入public文件夹，就是生成的网页资源静态文件
- 然后将这个文件夹推送到我们部署博客的git仓库
- 因为这里是CI线上的虚拟环境，不像我们本地配置了ssh密钥可以直接操作git仓库，所以需要用到token，token的使用就如GH_REF变量所示
- 最后是```master:main```意思是将本地的master分支推送到目标仓库的main分支的意思

{% endnote %}
