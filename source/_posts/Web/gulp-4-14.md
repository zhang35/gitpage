---
title: 搭建Gulp自动构建环境
date: 2018-04-14 15:02:21
tags: gulp
description: 使用gulp.js，实现web项目的自动生成，部署，浏览器自动刷新等
categories: 前端开发 
---

## 前言 
perl设计者在著作programming perl中提到:

优秀的程序员具有三大美德: 懒惰 急躁 和傲慢 ( laziness,Impatience.and Hubris)。

恩，第一就是懒，我十分认同。重复性工作全都应该交由机器去做。

于是在前端项目中，Gulp这种自动构建工具就应运而生了。

gulp通过定义任务，能完成前端项目的自动预处理、生成、部署等，甚至连刷新浏览器都省了，做到了“所见即所得”。

于是程序员能专注于coding，保存代码即可见到结果。

真是懒到极致了。

下面总结下Gulp项目的搭建流程。

## 什么是Gulp
> Gulp是一个自动化工具，前端开发者可以使用它来处理常见任务：
> - 搭建web服务器
> - 文件保存时自动重载浏览器
> - 使用预处理器如Sass、LESS
> - 优化资源，比如压缩CSS、JavaScript、压缩图片
> 当然Gulp能做的远不止这些。如果你够疯狂，你甚至可以使用它搭建一个静态页面生成器。Gulp真的足够强大，但你必须学会驾驭它。

<!-- more -->

## 安装gulp模块
使用npm安装gulp，以便以后能在终端使用gulp命令：
``` 
$ sudo cnpm install gulp -g
```

tip：安装淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm，提高模块下载速度：
```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```


## 创建gulp项目
### 创建package.json文件：
```
$ npm init
```
在 package.json 文件中指定项目依赖的包，以后可使用 npm install 命令一次性下载这些依赖包。
关于package.json详见：https://blog.csdn.net/u011240877/article/details/76582670

### 局部安装Gulp模块：
```
$ cnpm install gulp --save-dev
```
dependencies就是你程序运行需要的模块，没有这个模块你程序就会报错。
devDependencies是开发的时候需要的模块。
举个例子，你用angularjs框架开发一个程序，开发阶段需要用到gulp来构建你的开发和本地运行环境。所以angularjs一定要放到dependencies里，因为以后程序到生产环境也要用。gulp则是你用来压缩代码，打包等需要的工具，程序实际运行的时候并不需要，所以放到dev里就ok了。

### 创建gulpfile.js
```
var gulp = require('gulp'); //使用本地gulp模块

//任务模块
gulp.task('hi', function() {
    console.log('hello');
});
```

命令行执行：`$ gulp hello`，能看到：
```
[20:24:28] Starting 'hi'...
hello
[20:24:28] Finished 'hi' after 178 μ
```

### 写gulp任务
一般任务长这样：
```
gulp.task('task-name', function () {
  return gulp.src('source-files') // Get source files with gulp.src
    .pipe(aGulpPlugin()) // Sends it through a gulp plugin
    .pipe(gulp.dest('destination')) // Outputs the file in the destination folder
})
```
src输入源文件，pipe到插件处理后，输出到dest。

自动处理jade的任务：
```
var jade = require('gulp-jade');
 
gulp.task('templates', function() {
 
  gulp.src('/src/*.jade')
    .pipe(jade({
      pretty: true
    }))
    .pipe(gulp.dest('dist/'))
});
```
`pretty:true` 意思是对输出的html排版。不做设置的话，默认压缩html。

### 监视文件变动
监视文件变动代码如下：
```
gulp.watch('files-to-watch', ['tasks', 'to', 'run']);
```

或者使用通配符：
```
// **/*表示任意文件夹下的任意文件
gulp.watch('app/scss/**/*.scss', ['sass']);
```

或者使用任务同时监听多组文件：
```
gulp.task('watch', function(){
  gulp.watch('app/scss/**/*.scss', ['sass']);
  // Other watchers
})
```

### 使用livereload插件自动刷新浏览器
以监视html文件为例，一旦有变动，自动生成并刷新浏览器。
#### 本地安装gulp-livereload
`cnpm install gulp-livereload --save-dev`

#### 添加gulp任务
```
var gulp = require('gulp'),
    livereload = require('gulp-livereload');
 
gulp.task('watch', function() {
    livereload.listen();

    //监视src文件夹下所有文件
    gulp.watch('src/*.*', function(event) {  
        livereload.changed(event.path);  
    }); 
});
```

#### 安装浏览器插件
安装chrome的livereload插件，可下载crx文件直接拖入浏览器安装。

#### 以服务器方式打开页面
加载本地html文件无法触发livereload，必须以服务器方式加载。

这里使用node的超轻量级web服务器http-server。

安装http-server：`cnpm install http-server -g`

在html所在文件夹执行：`$ http-server`。

此时便能在http://localhost:8080/访问到页面。

#### 运行livereload
1、执行gulp任务：`$ gulp watch`

2、点击Chrome地址栏右边livereload按钮变成实心圈，即为启用。

此时修改文件，浏览器即可自动刷新。

### gulp + jade + livereload完整例子
gulpfile.js:
```
var gulp = require('gulp'),
  livereload = require('gulp-livereload'),
  jade = require('gulp-jade');

gulp.task('watch', function() {
  livereload.listen();
  gulp.watch('src/*.*', function(event) {  
    livereload.changed(event.path);  
  });  

  //监视jade文件修改，执行jade任务
  gulp.watch('src/*.jade', ['jade']);
});

gulp.task('jade', function() {
  gulp.src('src/*.jade')
  .pipe(jade(
    {pretty: true}
    ))
  .pipe(gulp.dest('dist/'))
});

gulp.task('default', ['watch']);
```
输入不带参数的命令：`$ gulp`，自动执行default任务。

实现自动处理jade，并自动刷新浏览器。
## 参考文章
https://www.cnblogs.com/Tom-yi/p/8036730.html
