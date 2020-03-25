---
title: Windows环境下，hexo+github搭建个人博客
date: 2019-07-10 15:12:28
tags:
 hexo、github、个人博客
---

# Windows环境下，hexo+github搭建个人博客
## 引言
**Hexo** 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

**Github page**是面向用户、组织和项目开放的公共静态页面搭建托管服务，站点可以被免费托管在[Github](https://github.com/) 上，你可以选择使用 Github Pages 默认提供的域名 github.io 或者自定义域名来发布站点，更便利地是你直接从你的GitHub存储库托管。只需编辑和推送你的blog，并且你的更改是实时的。（但由于网络的原因可能会稍微延时一点）

**优点**：操作简单，容易上手，完全免费；
**缺点**：更换电脑或操作系统后需要重新在本地搭建环境，不过博客还是那个博客。

## 总述
过程大致如下：

 1. 准备工作。
 2. 开始搭建。


### 准备工作

 1. **安装Node.js** 下载地址：https://nodejs.org/en/download  
 下载完成后一路默认安装即可。
 3. **安装Git软件** 下载地址：https://git-scm.com/download 
     下载完成后同样默认安装即可。
    安装完成后，点击鼠标右键，菜单会多处两个选项**Git GUI Here**和**Git Bash Here**。经常使用的是第二个.
    
3. **前往[Git官网](https://github.com)注册账号，创建新仓库**。
Repository name（仓库名）一定要是username.github.io的形式，比如我的github用户名是Samuelise，那么对应的仓库名就是 Samuelise.github.io ,同时这也是搭建的个人博客的域名。

OK，这样准备工作就做好了，现在我们开始搭建和部署环境。

### 开始搭建

 1. **安装hexo**。按住shift，点击鼠标右键，打开PowerShell窗口，输入

```
npm install -g hexo-cli
```

2. **初始化hexo**。在电脑选一个位置创建一个文件夹，比如我是在E盘创建了一个名为blog的文件夹。然后进入这个文件夹，点击鼠标右键，打开**git bash**窗口，输入

```
hexo init  #初始化命令
```

  接着，初始化完成后，安装依赖包，输入

```
hexo install  #安装依赖包
```

3. **测试生成结果**。输入

```
hexo g  #生成静态文件，等同于hexo generate
hexo s  #在本地服务器运行，等同于hexo server
```
生成完毕后，就可以打开浏览器，输入链接localhost:4000。效果如下： ![初始界面](https://img-blog.csdnimg.cn/20190710104408107.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0MDY0NDAw,size_16,color_FFFFFF,t_70)  4. **安装git部署插件**。在git bash输入命令

```
npm install hexo-deployer-git --save
```

6. **部署到Github上**。进入blog文件夹，打开其中的`_config.ym`文件，找到`#Deployment`起头的片段，按照下面的形式修改。(冒号后有一个空格）

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo:
    github: https://github.com/Samuellise/Samuelise.github.io.git,master
```
修改过后，在Git bash中输入

```
hexo g -d
```

上述工作完成后就可以通过域名username.github.io访问自己的博客了。



