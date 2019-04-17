# Gulp

gulp是一个基于流的构建工具，可以自动执行指定的任务

1. 开发环境下，想要能够按模块组织代码，监听实时变化

2. css/js预编译，postcss等方案，浏览器前缀自动补全等

3. 条件输出不同的网页，比如app页面和mobile页面

4. 线上环境下，我想要合并、压缩 html/css/javascritp/图片，减少网络请求，同时降低网络负担



`gulp.task(name[, deps], fn)`

定义任务的名字 name，依赖任务数组 deps，以及回调 fn。

gulp会先执行任务数组，结束后调用定义的函数



`gulp.src(globs[,options])`

返回一个可以传递给插件的数据流

- `app.js` 精确匹配
- `*.js` 能匹配js后缀的文件
- `**/*.js` 能匹配多级目录下的js文件（也包含当前目录下）
- `!js/app.js` 精确排除



`gulp.dest(path[, options])`

写文件，path 为最终输出的**目录名**，而非文件名



`gulp.watch(globs,[,opts], cb/tasks)`

监听文件变化，返回watcher对象

```js
gulp.task('watch', function (event) {
  gulp.watch('templates/*.tmpl.html', ['artTemplate']);
  console.log('Event type: ' + event.type); // added, changed, or deleted   
  console.log('Event path: ' + event.path); // The path of the modified file
});
```