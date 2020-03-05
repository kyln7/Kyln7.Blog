---
title: hexo+github+vscode  搭建个人博客
date: 2020-01-04 16:08:40
tags: [Blog]
---

---
# 准备工作
**win10系统**
1. 安装vscode git 和 nodejs

    安装方式可以看官方教程或其他教学，以下只介绍我遇到的一些问题。

    vscode设置默认终端为git bash：打开setting.json添加如下代码行（填入自己bash.exe的位置）
    {% asset_img vsset1.PNG %}
    ```
    {
    "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe",
    }
    ```

    注意在使用vscode时，可能会发生无法识别命令的情况
    {% asset_img 2.PNG %}（比如你输入的是node -v）
    可能的情况是：
    1.你在运行vscode时进行git/nodejs的安装，重启vscode或电脑
    2.你的git/nodejs安装成功但是并没有成功添加环境变量，请手动添加环境变量
    3.你可能需要使用管理员身份运行vscode    {% asset_img 3.PNG %}
    4.若以上方法都无效 将你的vscode setting.json 中所有代码删除（我怀疑手动设置vscode的terminal环境会导致冲突）

---
---
# 创建博客
1. 安装hexo：打开命令行输入 `npm install hexo-cli -g` {% asset_img 4PNG.PNG %}
2. 进入你想要存放blog的本地文件夹，打开vscode 在终端输入：
    ```
    hexo init blog
    cd blog
    npm install
    hexo server
    ```
    {% asset_img 5.PNG %}
    注意：一定要进入你创建的blog文件夹执行`npm install` 和  `hexo server`
    执行hexo server 后 可以在[本地](http://localhost:4000)中看到默认的博客
    {% asset_img 7.PNG %}

---
---
# 关联github
1. 根据官方文档创建repository 
    [官方文档](https://help.github.com/cn/github/working-with-github-pages/creating-a-github-pages-site)
2. 安装hexo-deployer-git插件 `npm install hexo-deployer-git --save` {% asset_img 8.PNG %}
3. 修改配置(_config.yml中找到deploy) {% asset_img 9.PNG %}
    ```
    deploy:
        type: git
        repo: <repository url> #https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
        branch: [branch]
        message: [message]
    ```
    url在你自己的repository中找到{% asset_img 10.PNG %}
---
---
# 设置子域名(使用子域名访问你的blog)
1. 购买一个域名，我是在腾讯云上买的
2. 在你的repository中点击setting找到github pages 设置你自己的域名 {% asset_img 6.PNG %}
3. 在你的repository中新建一个叫CNAME的文件{% asset_img 11.PNG %}文件中写下你自己购买的域名(我的是kyln7.com)
4. 安装BIND(域名系统，需要用到他的dig函数) [bind](https://www.isc.org/bind/) 
    windows系统是一个解压包，解压后直接把包目录添加到环境变量即可使用dig命令
5. 在命令行中输入` dig WWW.EXAMPLE.COM +nostats +nocomments +nocmd `(其中WWW.EXAMPLE.COM是你自己购买的域名)
    {% asset_img 12.PNG %}
6. 你可以发现自己的github.io.的ip 进入你购买的域名服务器商网站，找到域名解析设置，添加网站解析
    {% asset_img 13.PNG %}
等待一段时间，就能看到你的网站了。
---
