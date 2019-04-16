# Gulp

gulp是一个基于流的构建工具，可以自动执行指定的任务

1. 开发环境下，想要能够按模块组织代码，监听实时变化

2. css/js预编译，postcss等方案，浏览器前缀自动补全等

3. 条件输出不同的网页，比如app页面和mobile页面

4. 线上环境下，我想要合并、压缩 html/css/javascritp/图片，减少网络请求，同时降低网络负担



`gulp.task(name[, deps], fn)`