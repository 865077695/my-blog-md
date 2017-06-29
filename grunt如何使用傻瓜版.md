---
title: grunt如何使用
date: 2016-11-09 09:56:59
tags: Skill
---

简单地说，使用grunt能够提高效率，它带有代码压缩、代码合并、图片压缩、精灵图、less编译、sass编译、js语法检测等非常多的功能，而大部分新手死在了配置grunt的路上。
下面仔细写下我从0学习grunt整个过程的步骤并附带我总结的带有常用功能的配置文件。
需要工具：node环境（这个就跳过了）。
下面开始：
你必须已经装了node。
首先我们需要装上grunt
```html
$ npm install -g grunt-cli
```
上述命令执行完后，grunt 命令就被加入到你的系统路径中了，以后就可以在任何目录下执行此命令了。

注意，安装grunt-cli并不等于安装了 Grunt！Grunt CLI的任务很简单：调用与Gruntfile在同一目录中 Grunt。这样带来的好处是，允许你在同一个系统上同时安装多个版本的 Grunt。
这样就能让多个版本的 Grunt 同时安装在同一台机器上。


* 首先我们创建自己的项目根目录，这里我命名mypro，并在这个文件夹内创建两个文件和两个文件夹，如下图所示：
![](http://a2.qpic.cn/psb?/V10cPJls4bfSYP/nrvq23RKMZOtzKFlIP.SkVFGyrcvMvaJffugsl73J0k!/b/dK8AAAAAAAAA&bo=kgF8AQAAAAADAMs!&rf=viewer_4)

这个图里可以看到两个文件和两个文件夹，
package.json：此文件被npm用于存储项目的元数据，以便将此项目发布为npm模块。你可以在此文件中列出项目依赖的grunt和Grunt插件，放置于devDependencies配置段内。
Gruntfile: 此文件被命名为 Gruntfile.js，用来配置或定义任务（task）并加载Grunt插件的。
public文件夹：用来放置我们手写的，未经过压缩合并等操作的代码，可以和我一样在里面放img、js、css三个文件夹，然后分别存放相关文件。
build文件夹，这个是存放经过一些我们需要的操作之后生成的代码，上线使用。如果你对Gruntfile文件做了和我的一样的配置，那么这个文件夹在我们执行grunt时会自动生成。
* 下面分别解释一下package.json和Gruntfile两个文件。
你可以直接打开package.json将以下代码复制进去(**注意一定要把后面注释的内容全部去掉**)。
```json
{
  "name": "test",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "^0.4.5",
    "grunt-contrib-clean": "^1.0.0",      /*清理文件或文件夹*/
    "grunt-contrib-concat": "^1.0.1",     /*文件拼接（可将多个文件合并到一个文件）*/
    "grunt-contrib-cssmin": "^1.0.2",     /* 压缩css文件 */
    "grunt-contrib-imagemin": "^1.0.1",   /* 图片压缩 */
    "grunt-contrib-jshint": "^0.10.0",    /* 检测js语法错误 */
    "grunt-contrib-nodeunit": "~0.4.1",   /* 运行Nodeunit单元测试（NodeUnit：Node.js单元测试框架）*/
    "grunt-contrib-uglify": "^0.5.1",     /* 用UglifyJS方式压缩JS文件 */
    "grunt-contrib-watch": "^1.0.0",      /* 实时监测*/
    "grunt-spritesmith": "^6.3.1"         /* 雪碧图 */
  }
}
```

关于上面内容的解释：name和version我们不用管，最重要的是devDependencies里的内容，这里面写的是我们想要添加的功能，我把每个依赖的功能说明写上了，每个人可以根据自己的需求添加或移除一些功能。但json好像不支持这样注释，所以使用的时候一定记得把注释删掉。

然后在命令行进入项目根目录，即/mypro下，执行
```js
npm install
```

或者如果你被墙了就先执行
```js
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

切换淘宝源之后，执行
```js
cnpm install
```
当命令执行完成之后，在项目根目录mypro文件夹下会出现这样一个node_modules文件夹，里面便是自动下载的我们在package.json里面列出的所有devDependencies。此时目录结构如下图：
![](http://a3.qpic.cn/psb?/V10cPJls4bfSYP/dk*s*p9mCg.cdo0eM*B1B1nVTkBEVPozCFTUJeO5TpE!/b/dKcAAAAAAAAA&bo=8QHGAQAAAAADBxU!&rf=viewer_4)

至此package.json就可以放一边不用管了。

* 接着开始配置Gruntfile.js
这里我直接把我用的贴出来，然后仔细标注一下关键位置代码的含义，然后如果有需要其他功能的可以安装自己需要的功能，然后依例进行配置。
```js
/**
* Created by zzq on 2016/10/9.
*/
module.exports = function(grunt){
  grunt.initConfig({

    /**
    * clean清理
    * 安装npm install grunt-contrib-clean --save-dev
    * 运行grunt-clean
    /
    clean: {  // Grunt任务开始前的清理工作(清理build文件夹)
      files: ['build']
    },

    /**
    * css精灵
    * 安装npm install grunt --save-dev
    * 安装npm install grunt-spritesmith --save-dev
    * 运行grunt sprite
    */
    sprite:{
      devMove:{
        src:['public/images/*.png','public/images/*.jpg','!public/images/banner.png'],//所有的jpg和png图，除banner.png
        dest:'public/images/spriteRes.png',
        destCss:'public/css/spriteRes.css'
      }
    },

    /**
     * imagemin 压缩图片
     * 安装 npm install grunt-contrib-imagemin --save-dev
     * 运行 grunt imagemin
     */
    imagemin:{
      release:{
        files:[{
          expand:true,                          //
          src:['public/images/spriteRes.png','public/images/banner.png']
        }],
        options:{
          optimizationLevel:3               //压缩等级1-7
        }
      }
    },

    /**
     * cssmin 压缩css
     * 安装 npm install grunt-contrib-cssmin --save-dev
     * 运行 grunt cssmin
     */
    cssmin:{
      target:{
        files:[{
          expand:true,
          cwd:'public/css',
          src:['*.css'],
          dest:'build/css',
          ext:'.min.css'
        }]
      }
    },

    /**
     * jshint 校验js文件
     * 安装 npm install grunt-contrib-jshint
     * 运行 grunt jshint
     */
     // jshint:['Gruntfile.js'],
    jshint: { //文件校验，范例中未启用
      hint:{
        files:[{
          expand:true,
          cwd:'public/js',
          src:['*.js','! *.min.js'],
        }]
      }
    },

     /**
      *concat 合并css  js
      * 安装 npm install grunt-contrib-concat
      * 运行 grunt concat
      */
     concat: { //文件合并
      js_and_css: {
        files: {
          // js文件合并         这里分号左侧写合并后生成的文件及他的位置，右侧为你想要合并的几个文件
          'build/js/123.js': ['public/js/1.js', 'public/js/2.js', 'public/js/3.js'],
          'build/js/456.js' : ['public/js/4.js', 'public/js/5.js','public/js/6.js'],

          // css文件合并
          'build/css/style.css': ['public/css/1.css', 'public/css/2.css', 'public/css/3.css']
        }
      }
    },

     /**
     * uglify 精简js
     * 安装 npm install grunt-contrib-uglify --save-dev    //通过这个命令安装的插件，会自动在package.json下添加依赖
     * 运行 grunt uglify
     */
    uglify:{
      target:{                          //这个target是我们下面添加watch监听的依据，指定子任务，调用可以是grunt uglify(执行uglify里面的全部任务),grunt uglify :target(执行uglify里面的target任务)
        files:[{                        
          expand:true,                  //expand参数为true来启用动态扩展，涉及到多个文件处理需要开启
          cwd:'public/js',              //(原生)指向的目录 是相对的,全称Change Working Directory更改工作目录
          src:['*.js'],                 //指向源文件，**是一个通配符，用来匹配public/js下的所有js文件
          dest:'build/js',              //用来输出结果文件的文件夹
          ext:'.min.js'                 //扩展名
          // 注：如果src: [ '**', '!**/*.styl' ]，表示除去.styl文件，！在文件路径的开始处可以防止Grunt的匹配模式
        }]
      }
    },

    /**
    * less 编译less文件
    * 安装npm install grunt-contrib-less
    * 运行 grunt less
    */
    less:{
      compile:{
        files:[{
          expand:true,
          cwd:'public/css',
          src:['*.less'],
          dest:'public/css',
          ext:'.css'
        }]
      }
    },

    /**
    * 安装 npm install grunt-contrib-watch
    * watch 文件监听
    * 运行grunt watch
    */
    watch:{                 //实时监听，运行grunt watch之后，每次文件改变便会执行下面四个任务
        /**
        * 对一下四个方面做了实时监控
        * less实时编译、public/js下代码压缩至build/js内、
        * public/css下.less转译为.css仍放在public/css目录下、
        * public/css下css压缩至build/css内、
        * public/js下所有js文件语法检测
        */
      less:{
        tasks:['less:compile'],
        files:['public/css/*.less']
      },
      uglify:{
        tasks:['uglify:target'],
        files:['public/js/*.js']
      },
      cssmin:{
        tasks:['cssmin:target'],
        files:['public/css/*.css']
      },
      jshint:{
        tasks:['jshint:hint'],
        files:['public/js/*.js']
      }
    }

  });


  // 加载任务（分开加载）-每个上面有的功能都需要添加任务才能起到作用
  grunt.loadNpmTasks('grunt-contrib-cssmin');
  grunt.loadNpmTasks('grunt-contrib-clean');
  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-concat');
  grunt.loadNpmTasks('grunt-contrib-uglify');
  grunt.loadNpmTasks('grunt-contrib-watch');
  grunt.loadNpmTasks('grunt-contrib-less');
  grunt.loadNpmTasks('grunt-spritesmith');

  // 把grunt-contrib插件全部一次性加载
  // grunt.loadNpmTasks('grunt-contrib');

//注册默认任务，当我们执行grunt时会自动执行下面数组内的四个任务
  grunt.registerTask('default',['jshint','cssmin','concat','uglify']);
};
```
以上，便配置完成，如果还不会，那就按照第二幅图建好目录，然后把package.json和Gruntfile内容复制进去，命令行执行`$ npm install`,执行`$ grunt watch`,然后在public/js和public/css下添加一些文件看效果吧。

`$ grunt watch`添加了以下功能
    * less实时编译、public/js下代码压缩至build/js内、
    * public/css下.less转译为.css仍放在public/css目录下、
    * public/css下css压缩至build/css内、
    * public/js下所有js文件语法检测
`$ grunt` 添加了一下功能：
    * 代码压缩
    * js语法检测
    * css压缩
    * 文件合并



### grunt常用插件介绍：
1. grunt-contrib-clean (v0.5.0)
    清理文件或文件夹
2. grunt-contrib-coffee (v0.7.0)
    编译coffee文件为javascript文件
3. grunt-contrib-compass (v0.6.0)
    采用compass方式编译sass文件
4. grunt-contrib-compress (v0.5.2)
    压缩文件或文件夹
5. grunt-contrib-concat (v0.3.0)
    文件拼接（可将多个文件合并到一个文件）
6. grunt-contrib-copy (v0.4.1)
    复制文件或文件夹
7. grunt-contrib-cssmin (v0.6.2)
    压缩CSS文件
8. grunt-contrib-csslint (v0.1.2)
    CSS文件语法检查
9. grunt-contrib-htmlmin (v0.1.3)
    压缩HTML文件
10. grunt-contrib-imagemin (v0.3.0)
    PNG、JPEG图片压缩（保证质量压缩）
11. grunt-contrib-jshint (v0.6.4)
    JS语法检查
12. grunt-contrib-less (v0.7.0)
    将LESS编译成CSS
13. grunt-contrib-sass (v0.5.0)
    把SASS编译成CSS
14. grunt-contrib-stylus (v0.8.0)
    把Stylus文件编译成CSS
15. grunt-contrib-uglify (v0.2.4)
    用UglifyJS方式压缩JS文件
16. grunt-contrib-watch (v0.5.3)
    实时监测文件的增删改状态，状态改变时自动执行预定义任务
17. grunt-contrib-yuidoc (v0.5.0)
    编译YUIDoc文档
18. grunt-contrib-connect (v0.5.0)
    启动一个web服务器连接
19. grunt-contrib-jade (v0.8.0)
    编译Jade模版
20. grunt-contrib-handlebars (v0.5.11)
    预编译Handlebars模板到JST文件（Handlebars：结合json数据的模版）
21. grunt-contrib-jasmine (v0.5.2)
    通过PhantomJS运行jasmine（PhantomJS：JS单元测试）
22. grunt-contrib-jst (v0.5.1)
    预编译Underscore模板到JST文件（Underscore：JS工具库）
23. grunt-contrib-nodeunit (v0.2.1)
    运行Nodeunit单元测试（NodeUnit：Node.js单元测试框架）
24. grunt-contrib-qunit (v0.3.0)
    用PhantomJS对象运行QUnit单元测试
25. grunt-contrib-requirejs (v0.4.1)
    用r.js优化RequireJS项目


插件安装之后，可在node_modules文件夹中找到相应的插件（因为基于node，所以不用指定插件的路径也可以加载到插件，无论层级目录多深）。对应插件的语法可在对象的文件夹中查找README.md查看语法，有很多例子，需要注意的是对于多个文件的写法，比如less就需要注意，使用dynamic_mappings

更多插件访问（http://www.gruntjs.net/plugins）