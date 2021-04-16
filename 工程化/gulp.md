[TOC]

# gulp

Gulp.js 是一个自动化构建工具，开发者可以使用它在项目开发过程中自动执行常见任务。Gulp.js 是基于Node.js 构建的，利用Node.js 流的威力，开发者可以使用它在项目开发过程中自动执行常见任务。例如：css、js的合并与压缩（减少http请求，缩小文件大小）、html压缩、md5名生成与替换（一般解决浏览器缓存）、线上配置文件自动替换、搭建本地web服务器做到实时刷新等。

官方文档：[链接](https://www.gulpjs.com.cn/docs/getting-started/quick-start/)



## 安装

```bash
npm install --save-dev gulp
```



## Gulpfile 详解

gulpfile 是项目目录下名为 `gulpfile.js` （或者首字母大写 `Gulpfile.js`，就像 Makefile 一样命名）的文件，在运行 `gulp` 命令时会被自动加载。在这个文件中，你经常会看到类似 `src()`、`dest()`、`series()` 或 `parallel()` 函数之类的 gulp API，除此之外，纯 JavaScript 代码或 Node 模块也会被使用。任何导出（export）的函数都将注册到 gulp 的任务（task）系统中。



示例如下，gulpfile.js：

```js
const { src, dest, series, watch } = require('gulp')
const del = require('del')
// 引用后 gulp-uglify => plugins.uglify = require('gulp-uglify')，gulp- 前缀的包才生效
const plugins = require('gulp-load-plugins')()
// 网页热更新
const browserSync = require('browser-sync').create()
const reload = browserSync.reload

// 对scss/less编译，压缩，输出css文件
function css(cb) {
  src('css/*.scss')
    // 下一个处理环节
    .pipe(plugins.sass({
      outputStyle: 'compressed'
    }))
    .pipe(plugins.autoprefixer({
      cascade: false,
      remove: false
    }))
    .pipe(dest('./dist/css'))
    // 刷新页面
    .pipe(reload({
      stream: true
    }))
  // 告知异步任务执行完成，否则报错
  cb()
}

// 压缩js uglifyjs
function js(cb) {
  src('js/*.js')
    // 下一个处理环节
    .pipe(plugins.uglify())
    .pipe(dest('./dist/js'))
    // 刷新页面
    .pipe(reload({
      stream: true
    }))
  // 告知异步任务执行完成，否则报错
  cb()
}

// 监听文件变化
function watcher() {
  watch('js/*.js', js)
  watch('css/*.scss', css)
}

// 删除 dist 目录中的内容
function clean(cb) {
  del('./dist')
  cb()
}

// server 任务
function serve(cb) {
  browserSync.init({
    server: {
      baseDir: './'
    }
  })
  cb()
}

// 使用 gulp xxx ，执行对应的任务
exports.css = css
exports.js = js
exports.watcher = watcher
exports.clean = clean
exports.serve = serve

// 使用 gulp ，不需要额外指定任务名，执行 default 任务
exports.default = series([clean, js, css, serve, watcher])
```

