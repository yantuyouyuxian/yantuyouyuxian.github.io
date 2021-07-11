---
title: 如何在hexo的博客中插入图片
date: 2021-06-15 00:23:26
categories:
- hexo
tags: 
- hexo
cover: false
---

# 如何在hexo的博客中插入图片

- 首先打开hexo的配置文件，就是生成博客根目录中的_config.yaml

- 找到post_asset_folder设置项,将其设置为true

  ```yaml
  post_asset_folder: true
  ```

- 这样,我们每次使用hexo命令新建一篇文章的时候hexo都会帮我们生成一个同名的文件夹

- 因为生成的文章和这个同名文件夹最终会合并到一起,所以在文章中直接引用这个文件夹中图片就行了

## 举例:

比如我在同名文件夹下放了一张图片：2.jpeg，在文章中：

1. 使用标签（typora无法预览，但有效）

   ```java
   // {% asset_img 2.jpeg img %}
   ```
   {% asset_img 2.jpeg img %}

3. 直接文件夹\图片名（typora可以预览，但无效）

   ```java
   //![img](”和文章标题同名的文件夹名称“\2.jpeg)
   ```

   ![](hello-world\2.jpeg)

4. 直接使用图片名（typora中无法预览，但有效）

   ```java
   //![img](2.jpeg)
   ```

   ![img](2.jpeg)
   
4. 直接访问img文件夹下的图片（typora无法预览，但有效）

   ```java
   //![img](/img/b.jpeg)
   //如果在img下确实有这张图片是可以显示的
   ```
   
   ![img](/img/b.jpeg)

# 结论

- 观察hexo生成的网页资源文件目录，就明白了。











