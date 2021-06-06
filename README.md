

<p align='center'>
    <img src="https://badgen.net/badge/labels/8"/>
    <img src="https://badgen.net/github/issues/Sweet-KK/blog"/>
    <img src="https://badgen.net/badge/last-commit/2021-06-06 08:47:23"/>
    <img src="https://badgen.net/github/forks/Sweet-KK/blog"/>
    <img src="https://badgen.net/github/stars/Sweet-KK/blog"/>
    <img src="https://badgen.net/github/watchers/Sweet-KK/blog"/>
    <img src="https://badgen.net/github/release/Sweet-KK/blog"/>
</p>

<p align='center'>
    <a href="https://github.com/jwenjian/visitor-count-badge">
        <img src="https://visitor-badge.glitch.me/badge?page_id=sweet_kk.blog"/>
    </a>
</p>


## 置顶 :thumbsup: 
- [迁移ing](https://github.com/Sweet-KK/blog/issues/2)  <sup>0 :speech_balloon:</sup>  	 
## 最新 :new: 

#### [使用node发送验证码到手机或邮箱做校验](https://github.com/Sweet-KK/blog/issues/11) <sup>0 :speech_balloon:</sup> 	 2021-06-06 08:45:52

:label: : [node](https://github.com/Sweet-KK/blog/labels/node)

date: 2018-08-27


最近做的一个node项目，有个密码找回的功能，需要用到短信验证，后来又因为某种原因，要改成邮箱发送信息~类似这种验证码的需求想必是很常见的，只要有用户注册、密码修改找回功能的基本上都会用到，今天就介绍一下发送短信和发送邮件验证的基本步骤。

### 短

[更多>>>](https://github.com/Sweet-KK/blog/issues/11)

---


#### [vue-cli 2.0再优化](https://github.com/Sweet-KK/blog/issues/10) <sup>0 :speech_balloon:</sup> 	 2021-06-06 08:43:39

:label: : [前端](https://github.com/Sweet-KK/blog/labels/%E5%89%8D%E7%AB%AF)

date: 2018-12-19

由于公司电脑实在太水了，打开个项目，代码越多越卡顿，找运维加内存就一直拖，唉~ 只能自己花时间去优化项目，减少代码，并且寻找钻研提高webpack构建速度的方法（初始无优化构建时间140+秒，优化后构建50+秒，最快22+秒）。



### 一、vu

[更多>>>](https://github.com/Sweet-KK/blog/issues/10)

---


#### [vue-cli踩坑与优化小记](https://github.com/Sweet-KK/blog/issues/9) <sup>0 :speech_balloon:</sup> 	 2021-06-06 08:40:46

:label: : [前端](https://github.com/Sweet-KK/blog/labels/%E5%89%8D%E7%AB%AF)

date: 2018-08-17


之前用vue-cli写过项目，但是隔了一段时间再次接触的时候，发现很多都忘了，翻了一下自己笔记，写的都是无关紧要的东西，完完全全可以在官方文档查到。这次开发遇到了很多问题，记忆中有些许印象，但就是想不起来，搜索、解决问题的时间就占去了很多，为了方便以后开发

[更多>>>](https://github.com/Sweet-KK/blog/issues/9)

---


#### [Grid VS Flexbox](https://github.com/Sweet-KK/blog/issues/8) <sup>0 :speech_balloon:</sup> 	 2021-06-06 08:36:56

:label: : [前端](https://github.com/Sweet-KK/blog/labels/%E5%89%8D%E7%AB%AF)

date: 2018-03-04

对于Web开发者来说，网页布局一直是个比较重要的问题。但实际上，在网页开发很长的一段时间当中，我们甚至没有一个比较完整的布局模块。总的来说 Web 布局经历了四个阶段......


#### 一、前言

> 原文链接：[blog.catwen.cn

[更多>>>](https://github.com/Sweet-KK/blog/issues/8)

---


#### [静态页面HTML代码的复用实践](https://github.com/Sweet-KK/blog/issues/7) <sup>0 :speech_balloon:</sup> 	 2021-06-06 08:35:00

:label: : [前端](https://github.com/Sweet-KK/blog/labels/%E5%89%8D%E7%AB%AF)

date: 2018-05-19


最近做的一些页面，纯粹使用jq这种，没有一些组件复用的功能，又需要经常修改，例如页面顶部和底部这些区域，相同的代码，却需要一个个页面打开去修改a标签的跳转、文件的引用路径等等，页面层级不一样还要注意是../还是./。一旦改错了，就等于做了无用功，回头还要一个个去检查。这样耗费的时间和精力实在是太多了。因此就有了本文的思考，很幸运目前已有gulp-file-include这样的插件去实现html的复用，减少我们的修改维护成本。以下就是介绍本实践项目的目录结构与gulp-file-include的使用



### 目录结构

![image.png](https://upload-images.jianshu.io/upload_images/8192053-6c064e4b9406b820.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### RUN DEMO

```
# 1.进入项目根目录

# 2.安装依赖
npm install

# 运行服务编译文件
gulp  
编译完成浏览器自动打开入口链接页面
```



### gulpfile.js

```
var gulp = require('gulp');
var browserSync = require('browser-sync');
var reload = browserSync.reload;
var fileinclude = require('gulp-file-include');
var imagemin = require('gulp-imagemin');
var less = require('gulp-less');
var autoprefixer = require('gulp-autoprefixer');

var browserArr = ['last 2 versions', 'ie 9'];

var app = {  // 定义目录
  srcPath: './src/',
  // buildPath:'./build/',
  distPath: './dist/'
}

// 处理公用html
gulp.task('fileinclude', function () {
  return gulp.src([app.srcPath + 'html/**/*.html'])
    .pipe(fileinclude({
      prefix: '@@',  // 自定义标识前缀
      basepath: app.srcPath + 'html/components'  // 复用组件目录
    }))
    .pipe(gulp.dest(app.distPath + 'html'));
});

// 处理完lib文件后返回流
gulp.task('lib', function () {
  return gulp.src(app.srcPath + 'lib/**/*')
    .pipe(gulp.dest(app.distPath + 'lib'));
});

// 处理完JS文件后返回流
gulp.task('js', function () {
  return gulp.src(app.srcPath + 'js/**/*.js')
    .pipe(gulp.dest(app.distPath + 'js'));
});
// 处理less文件后返回流
gulp.task('less', function () {
  return gulp.src(app.srcPath + 'less/**/*.less')
    .pipe(less())
    .pipe(autoprefixer({
      browsers: browserArr,
      cascade: true, //是否美化属性值 默认：true 像这样：
      //-webkit-transform: rotate(45deg);
      //        transform: rotate(45deg);
    }))
    .pipe(gulp.dest(app.distPath + 'css'))
    .pipe(reload({stream: true}));
});
// 处理完CSS文件后返回流
gulp.task('css', function () {
  return gulp.src(app.srcPath + 'css/**/*.css')
    .pipe(autoprefixer({
      browsers: browserArr,
      cascade: true, //是否美化属性值 默认：true
    }))
    .pipe(gulp.dest(app.distPath + 'css'))
    .pipe(reload({stream: true}));
});

/*处理图片*/
gulp.task('image', function () {
  return gulp.src(app.srcPath + 'images/**/*')
    //.pipe(imagemin())
    .pipe(gulp.dest(app.distPath + 'images'))
});


// 创建一个任务确保JS任务完成之前能够继续响应
// 浏览器重载
gulp.task('lib-watch', ['lib'], reload);
gulp.task('js-watch', ['js'], reload);
gulp.task('img-watch', ['image'], reload);
gulp.task('html-watch', ['fileinclude'], reload);

//静态服务器+监听
gulp.task('server', ['js', 'css', 'image', 'lib', 'less'], function () {
  gulp.start('fileinclude');
  browserSync.init({
    port: 80,
    server: {
      baseDir: './',
    }
  });

  // 无刷新方式更新
  gulp.watch(app.srcPath + 'less/**/*.less', ['less']);
  gulp.watch(app.srcPath + 'css/**/*.css', ['css']);

  // 添加 browserSync.reload 到任务队列里
  // 所有的浏览器重载后任务完成。
  // 刷新页面方式
  gulp.watch(app.srcPath + 'lib/**/*', ['lib-watch']);
  gulp.watch(app.srcPath + 'js/**/*.js', ['js-watch']);
  gulp.watch(app.srcPath + 'images/**/*', ['img-watch']);
  gulp.watch(app.srcPath + 'html/**/*.html', ['html-watch']);
  // gulp.watch(app.distPath + 'html/**/*.html').on('change', reload)
})


/*默认任务*/
gulp.task('default', ['server']);
```



### 编写html页面示例

#### 1.导入复用组件

[更多>>>](https://github.com/Sweet-KK/blog/issues/7)

---


## 分类  :card_file_box: 

<details open="open">
    <summary>
        <img src="assets/wordcloud.png" title="词云, 点击展开详细分类" alt="词云， 点击展开详细分类">
        <p align="center">:cloud: 词云 :cloud: <sub>点击词云展开详细分类:point_down: </sub></p>
    </summary>


<details>
<summary>:+1: 置顶	<sup>1:newspaper:</sup></summary>

- [迁移ing](https://github.com/Sweet-KK/blog/issues/2)  <sup>0 :speech_balloon:</sup>  	 


</details>

<details>
<summary>:framed_picture: 封面	<sup>1:newspaper:</sup></summary>

- [Open the future](https://github.com/Sweet-KK/blog/issues/1)  <sup>0 :speech_balloon:</sup>  	 


</details>

<details>
<summary>java	<sup>0:newspaper:</sup></summary>



</details>

<details>
<summary>node	<sup>1:newspaper:</sup></summary>

- [使用node发送验证码到手机或邮箱做校验](https://github.com/Sweet-KK/blog/issues/11)  <sup>0 :speech_balloon:</sup>  	 


</details>

<details>
<summary>python	<sup>0:newspaper:</sup></summary>



</details>

<details>
<summary>前端	<sup>7:newspaper:</sup></summary>

- [vue-cli 2.0再优化](https://github.com/Sweet-KK/blog/issues/10)  <sup>0 :speech_balloon:</sup>  	 
- [vue-cli踩坑与优化小记](https://github.com/Sweet-KK/blog/issues/9)  <sup>0 :speech_balloon:</sup>  	 
- [Grid VS Flexbox](https://github.com/Sweet-KK/blog/issues/8)  <sup>0 :speech_balloon:</sup>  	 
- [静态页面HTML代码的复用实践](https://github.com/Sweet-KK/blog/issues/7)  <sup>0 :speech_balloon:</sup>  	 
- [干货!各种常见布局+知名网站实例分析+相关阅读推荐](https://github.com/Sweet-KK/blog/issues/5)  <sup>0 :speech_balloon:</sup>  	 
- [CSS预处理器--Less语法整理](https://github.com/Sweet-KK/blog/issues/4)  <sup>0 :speech_balloon:</sup>  	 
- [对constructor和prototype的理解](https://github.com/Sweet-KK/blog/issues/3)  <sup>0 :speech_balloon:</sup>  	 


</details>

<details>
<summary>开源	<sup>0:newspaper:</sup></summary>



</details>

<details>
<summary>服务器、运维	<sup>1:newspaper:</sup></summary>

- [CentOS 7.4 从0到1部署node项目](https://github.com/Sweet-KK/blog/issues/6)  <sup>0 :speech_balloon:</sup>  	 


</details>


</details>    
