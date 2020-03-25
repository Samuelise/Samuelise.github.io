---
title: hexo+github个人博客主题更换
date: 2019-07-11 19:11:59
tags:
 hexo、主题
---
# hexo+github个人博客主题更换
## 前言
我们已经通过使用**hexo+github**搭建起了自己的个人博客，但hexo默认的**landscape**主题实在是不怎么好看，不过Hexo为我们提供了免费下载主题的渠道：https://hexo.io/themes/  。

## 更换主题
以作者的博客为例。https://samuelise.github.io/ 我用的主题是这样的：
![博客](https://img-blog.csdnimg.cn/2019071117593515.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MDY0NDAw,size_16,color_FFFFFF,t_70) 
**首先，我们要下载一个喜欢的主题。**

进入[Hexo主题下载官网](https://hexo.io/themes/)挑选一个喜欢的主题。
![hexo主题](https://img-blog.csdnimg.cn/20190711180302285.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MDY0NDAw,size_16,color_FFFFFF,t_70) 


比如，我喜欢A-ACE这个主题，那么我点进去就是该主题作者的博客，同时也可以看到这个主题的博客是什么样的效果。
大多数情况下我们都可以通过其博客去访问该作者的Github账户。在其仓库就以找到主题设置文件（也可通过关键字进行搜索）。
![主题设置文件](https://img-blog.csdnimg.cn/20190711181845546.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MDY0NDAw,size_16,color_FFFFFF,t_70)

 打开这个项目，获取其Web链接。
![获取下载链接](https://img-blog.csdnimg.cn/20190711182245789.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MDY0NDAw,size_16,color_FFFFFF,t_70) 

获取成功后，回到我个人博客的根目录。
    ![根目录](https://img-blog.csdnimg.cn/20190711182946623.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MDY0NDAw,size_16,color_FFFFFF,t_70)     

打开Git Bash窗口。输入下面的指令：

```
cd themes   #进入主题文件夹
git clone https://github.com/kinggozhang/hexo-theme-ace.git  #下载主题相关文件
```

这样，我们就把该主题相关文件下载好了。
![okk](https://img-blog.csdnimg.cn/20190711184244509.PNG)

 **接下来，就是要改配置，使用我们下载的主题**。
打开博客根目录下的_config.yml文件，作者使用记事本打开的（大雾），推荐使用Notepad++，更改主题设置。改为我们下载的项目的名字。
![配置文件](https://img-blog.csdnimg.cn/20190711185605613.PNG)
 保存之后，在博客根目录下打开Git Bash，输入下面的指令：

```
hexo clean      #清空public缓存
hexo g          #重新生成
hexo s          #本地预览
```
打开浏览器，输入 http://localhost:4000/ 打开本地仓库，可以看到主题已经被更改了。
![更改成功](https://img-blog.csdnimg.cn/20190711190423760.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MDY0NDAw,size_16,color_FFFFFF,t_70) 
最后，我们只要上传到远程仓库就可以了。

```
hexo d      #上传
```

okk，这样，我们就将主题换成了我们喜欢的了。
























