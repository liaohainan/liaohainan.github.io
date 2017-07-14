---
title: gulp实现自动刷新页面+反向代理本地调试接口
date: 2016-12-10 20:36:24
tags: JS
---

使用browser-sync实现页面自动刷新
使用http-proxy-middleware实现反向代理功能

```js

var gulp = require('gulp')
var less = require('gulp-less')
var miniCss = require('gulp-clean-css')
var uglifyJs = require('gulp-uglify')
var revAppend = require('gulp-rev-append')
// var fileinclude = require('gulp-file-include');
var imagemin = require('gulp-imagemin');
var replace = require('gulp-url-replace');
var htmlmin = require('gulp-htmlmin');
var browserSync = require('browser-sync');
// var reload = browserSync.reload;
var proxyMiddleware = require('http-proxy-middleware');
var plugins = require('gulp-load-plugins')();


gulp.task('build', function () {
    //编译less
    gulp.src('dev/css/*.css')
        .pipe(gulp.dest('dist/css'));
    gulp.src('dev/css/*.less')
        .pipe(less())
        .pipe(miniCss())
        .pipe(gulp.dest('dist/css'));
    //压缩js，libs文件夹不做压缩
    gulp.src(['dev/js/*.js', '!dev/js/libs/*.js'])
        .pipe(uglifyJs())
        .pipe(gulp.dest('dist/js'));
    gulp.src('dev/js/libs/*.swf')
        .pipe(gulp.dest('dist/js/libs'));
    //不压缩的也复制过去
    gulp.src('dev/js/libs/*.js')
        .pipe(gulp.dest('dist/js/libs'));
// 适配page中所有文件夹下的所有html，排除page下的include文件夹中html，静态文件加版本号，替换静态文件目录

    //清除html的注释
    var options = {
        removeComments:true,
    };
    gulp.src(['dev/**.html', '!dev/include/**.html'])
        // .pipe(fileinclude({
        //     prefix: '@@',
        //     basepath: '@file'
        // }))
        .pipe(revAppend())
        .pipe(replace({
            'images/': 'images/',
            'css/': 'css/',
            'js/': 'js/',
        }))
        .pipe(htmlmin(options))
        .pipe(gulp.dest('dist'));


    //压缩图片
    gulp.src('dev/images/*.{png,jpg,gif,ico}')
        .pipe(imagemin())
        .pipe(gulp.dest('dist/images'));

});
gulp.task('build-dev', function () {
    //编译less
    gulp.src('dev/css/*.css')
        .pipe(gulp.dest('dist/css'));
    gulp.src('dev/css/*.less')
        .pipe(less())
        // .pipe(miniCss())
        .pipe(gulp.dest('dist/css'))
        // .pipe(reload({stream: true}));
    //压缩js，libs文件夹不做压缩
    gulp.src(['dev/js/*.js', '!dev/js/libs/*.js'])
        // .pipe(uglifyJs())
        .pipe(gulp.dest('dist/js'))
        // .pipe(reload({stream: true}));
    gulp.src('dev/js/libs/*.swf')
        .pipe(gulp.dest('dist/js/libs'));
    //不压缩的也复制过去
    gulp.src('dev/js/libs/*.js')
        .pipe(gulp.dest('dist/js/libs'));
// 适配page中所有文件夹下的所有html，排除page下的include文件夹中html，静态文件加版本号，替换静态文件目录
    gulp.src(['dev/**.html', '!dev/include/**.html'])
        // .pipe(fileinclude({
        //     prefix: '@@',
        //     basepath: '@file'
        // }))
        .pipe(revAppend())
        .pipe(replace({
            'images/': 'images/',
            'css/': 'css/',
            'js/': 'js/',
        }))
        .pipe(gulp.dest('dist'))
        // .pipe(reload({stream: true}));
    gulp.src('dev/*.jsp')
        .pipe(revAppend())
        // .pipe(replace({
        //     'images/': 'content/images/',
        //     'css/': 'content/css/',
        //     'js/': 'content/js/',
        // }))
        .pipe(gulp.dest('dist'))

    //压缩图片
    gulp.src('dev/images/*.{png,jpg,gif,ico}')
        // .pipe(imagemin())
        .pipe(gulp.dest('dist/images'))
        // .pipe(reload({stream: true}));

});
gulp.task('browserSync', function() {
    var middleware = proxyMiddleware('/companyService', {
        // target: 'http://10.2.54.5',
        target: 'http://10.2.54.17',
        // target: 'http://127.0.0.1:4000',
        changeOrigin: true,             // for vhosted sites, changes host header to match to target's host
        logLevel: 'debug',
        pathRewrite: {
            // '^/epay_www': '/epay_www/'
            // '^/epay_passport': '/epay_passport/'
            '^/companyService': '/companyService'
        }
    });
    /**
     * Add the proxy to browser-sync
     */
    browserSync.init({
        server: {
            baseDir: './dev/',
            port: 3000,
            middleware: [middleware],
        },
        startPath: '/'
    });
});
gulp.task('change',function(){
    console.log('have change');
})
gulp.task('watch', function() {
    // gulp.watch(['./src/**/*.js'], ['inject']);
    // gulp watch 无法监听增加文件和删除文件, 查看 github issue, 他们不准备fix了, 等4.0 呵呵吧
    // plugins.watch('dev/**/*.js', function() {
    //     gulp.run('build-dev');
    // });
    // plugins.watch('dev/**/*.css', function() {
    //     gulp.run('build-dev');
    // });
    gulp.watch('dev/**/*.+(less|js|html|css|jsp)', ['change'])
        .on('change', browserSync.reload);
});
gulp.task('dev', ['browserSync', 'watch']);
gulp.task('default', ['dev', 'build-dev']);

```