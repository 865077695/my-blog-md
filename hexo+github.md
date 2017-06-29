---
title: Hexo+GitHub
date: 2016-10-19 18:19:21
tags: Skill
---



### 环境：
 * 安装node（必须）
 * 安装git（必须）
 * github账号（必须）
 
### 开始安装hexo
* 首先全局安装hexo：

    npm install -g hexo
* 初始化hexo

    选择一个空目录，如my-blog，在该目录下执行`hexo init`
    
        hexo init
至此安装完毕
### 生成静态页面
    
    hexo generate   (或者简写hexo g)
### 本地启动

    hexo server     (简写hexo s)
此时可以看到在命令窗口有显示程序运行在localhost:4000.我们通过浏览器打开localhost:4000就可以看到页面了。然后就是怎么才能放在网上让别人也可以看到。
### 配置github
在github建立一个新的repository，并且命名为yourUserName.github.io，注意这个名字是固定写法。

现在打开my-blog根目录的_config.yml配置文件，拉到最先面配置deploy参数如下

    deploy:
      type: git
      repository: https://github.com/yourUserName/yourUserName.github.io.git
      branch: master
注意`:`后面的空格，以及repository末尾的`.git`

执行以下命令：
    
    npm install hexo-deployer-git --save
最后执行
    
    hexo deploy
进行部署。此时我们通过访问`https://yourUsername.github.io就可以看到部署的博客了。

### 部署步骤

要新建文章可以通过`hexo new 'particle_name'自动新建，在source下进行编辑之后通过以下命令部署：
    
    hexo clean      // 清除记录。
    hexo generate   // 生成静态文件。      简写：hexo g
    hexo deploy     // 部署至github。     简写：hexo d

教程里面的几个坑：
* 修改配置文件的第三步修改文件里面的deploy：type应为git而不是github
* 在使用命令`hexo g`和`hexo d`将代码部署到github之前，需要输入`npm install hexo-deployer-git --save`，然后再进行部署



## Tips
1. 编辑完之后，在命令行运行 `hexo s` 然后通过localhost:4000本地预览.
2. 本地预览通过后，在命令行依次运行 `hexo g` 和  `hexo d`上传到github。然后通过[xxx.github.io](https://865077695.github.io)访问（xxx为自己的github名字）

### 下载修改hexo主题
Hexo静态博客安装主题也容易，在Github上找到你喜欢的主题，然后执行类似命令：git clone https://github.com/heroicyang/hexo-theme-modernist.git themes/modernist
这时就将modernist主题下载下来了，打开hexo\_config.yml，修改主题为theme: modernist
