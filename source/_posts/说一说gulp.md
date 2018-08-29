---
title: 说一说gulp
date: 2018-08-29 23:21:47
tags: gulp
---

# 概览

一个完整的gulp任务流程，如下图所示：
![gulp任务流程图](https://oml33zwo9.qnssl.com//18-8-29/84442561.jpg)

# gulp.task

- **gulp.task(name[, deps], fn)**

- **name**: 任务名

- **deps**：一个包含任务列表的数组，这些任务会在你当前任务运行之前完成。

- **fn**: 该函数定义任务所要执行的一些操作。通常来说，它会是这种形式：gulp.src().pipe(someplugin())

        gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
            // 做一些事
        });

# gulp.src

- **gulp.src(globs[, options])**

        gulp.src('css/*.css')
                .pipe(cached('css'))
                .pipe(autoprefixer())
                .pipe(csscomb())
                .pipe(cleancss({
                    "keepBreaks": true,
                    "compatibility": 'ie7'
                }))
                .pipe(gulp.dest('./src/css/'))

- **globs**: 文件匹配规则，翻译为水滴；

- **options**:

![](https://oml33zwo9.qnssl.com//18-8-29/46007188.jpg)

下面是options.base的示例：

    ```js
    gulp.src('app/css/*.css')
            .pipe(autoprefixer())
            .pipe(csscomb())
            .pipe(gulp.dest('dist')); // dist/*.css
    
    gulp.src('app/css/*.css', { base: 'app' })
            .pipe(autoprefixer())
            .pipe(csscomb())
            .pipe(gulp.dest('dist')); // dist/css/*.css
    ```

# gulp.dest

- **gulp.dest(path[, options])**

    > 能被 pipe 进来，并且将会写文件。并且重新输出（emits）所有数据，因此你可以将它 pipe 到多个文件夹。如果某文件夹不存在，将会自动创建它。

        gulp.src('*.html')
            .pipe(cached('html'))
                .pipe(gulp.dest('./src/'))

- **path**: 文件将被写入的路径（输出目录）。也可以传入一个函数，在函数中返回相应路径

- **options**：

![](https://oml33zwo9.qnssl.com//18-8-29/67661714.jpg)


# gulp.watch

- **gulp.watch(glob[, opts], tasks)**
- **glob**: 用来指定具体监控哪些文件的变动。
- **opts**: 主要用来配置监听哪种类型的变动，例如：added、changed、deleted、all
- **tasks**: 需要在文件变动后执行的一个或者多个通过 gulp.task() 创建的 task 的名字

        var watcher = gulp.watch('less/*.less', ['css']);
        //下面这段
        watcher.on('change', function(event) {
        console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
        });

- **gulp.watch(glob[, opts, cb])** *这里不做详解，可参考gulp API文档*