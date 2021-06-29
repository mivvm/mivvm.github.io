---
title: gulp4与gulp3
date: 2019-08-11 18:09:51
tags: [gulp, js, css]
categories: 
- gulp
---

### 踩坑记录

node12+以上的版本不兼容gulp3版本，gulp3最好使用node10的版本，gulp4最好使用node13+的版本

### 安装参照官网

###	Task

gulp版本导致，task可以分为两种：

+ gulp3使用的

```js
var gulp = require("gulp");
var uglify = require("gulp-uglify");// js压缩
gulp.task("minify-js", function () {
    gulp.src(['Public/static/**/*.js', '!Public/static/**/*.min.js'])
    .pipe(uglify())
    .pipe(gulp.dest('Public/Template'));
});
```

gulp3的时候可以直接使用**gulp minify-js**来调用任务 

+ gulp4使用的

```js
const { src, dest, watch, series, parallel} = require('gulp');
const uglify = require("gulp-uglify");// js压缩
function MinJs() {
    return src(["assets/js/**/*.js", "!assets/js/*.min.js"])
    // 匹配要处理的文件
        .pipe(babel({
            presets: ['@babel/env']
        }))
        .pipe(rename({suffix: ".min"}))
        // 添加文件后缀名
        .pipe(uglify())
        // 压缩js文件
        .pipe(dest("assets/minjs"));
    // 输出

}
exports.js = MinJs
```

需要使用exports，在**gulp js**调用

### 多任务的处理

+ gulp3处理

```js
// 默认任务
gulp.task("default", [task1, task2], function () {
    livereload.listen();
    gulp.src("./*").pipe(livereload());
    // 有疑惑
});
```

直接**gulp default**调用

+ gulp4处理
  + **series**：序列（顺序执行）
  + **parallel**：并行（同时执行）
  + 混用

```js
exports.taskName = series(task1, task2)
exports.taskName = parallel(task1, task2)
exports.taskName = series(clean, parallel(css, javascript))
```

### 监听Watch

- gulp3

```js
//最好将所有的任务都放到default任务里面，最好watch调用default就好了
gulp.task('watch', function () {
    gulp.watch(["Public/static/**/*.js", "Public/static/**/*.css"], ["default"]);
});
gulp watch调用
```

+ gulp4

```js
function watchTask() {
    watch(['assets/sass/*', 'assets/js/*', 'assets/js/**/*.js'], parallel(task1, task2, 	task3))
}
exports.watch = watchTask
gulp watch调用
```

至此可以实现文件的热更新，编译，**官网介绍很清楚**特别是 [首页的链接](https://www.gulpjs.com.cn/ )

