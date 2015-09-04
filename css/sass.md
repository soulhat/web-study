# Sass
#### Mac安装
```
// 前提安装了ruby，检测
ruby －v
// gem仓库站点 https://rubygems.org
sudo gem install sass
// 如果被墙，可直接下载后再安装
sudo gem install path/sass-3.14.18.gem
// 检查安装版本
sass -v
```
#### 更新
```
gem update sass
```
#### 卸载
```
gem uninstall sass
```
#### 编译
* 命令编译

```
// 单文件编译
sass <path>/style.scss:<path>/style.css
// 多文件编译
sass <path>/sass/:<path>/css/
// sass --watch
sass --watch test.sass:test.css
sass --watch test.scss:test.css
```
* GUI工具编译
  * Koala (http://koala-app.com/)
  * Compass.app（http://compass.kkbox.com/）
  * Scout（http://mhs.github.io/scout-app/）
  * CodeKit（https://incident57.com/codekit/index.html）
  * Prepros（https://prepros.io/）
* 自动化编译
  * grunt
  
```
module.exports = function(grunt) {
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        sass: {
            dist: {
                files: {
                    'style/style.css' : 'sass/style.scss'
                }
            }
        },
        watch: {
            css: {
                files: '**/*.scss',
                tasks: ['sass']
            }
        }
    });
    grunt.loadNpmTasks('grunt-contrib-sass');
    grunt.loadNpmTasks('grunt-contrib-watch');
    grunt.registerTask('default',['watch']);
}
```
  * gulp

```
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function () {
    gulp.src('./scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./css'));
});

gulp.task('watch', function() {
    gulp.watch('scss/*.scss', ['sass']);
});

gulp.task('default', ['sass','watch']);
```

## Sass与Scss的区别
.sass只能使用Sass老语法规则（严格tab缩进规则，没看到类似 CSS 中的大括号和分号）

.scss使用的是Sass的新语法规则，也就是 SCSS 语法规则（类似 CSS 语法格式，代码都包裹在一对大括号里，并且末尾结束处都有一个分号）
```
// css样式
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
```
```
// Sass实现
$font-stack: Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```
```
// Scss实现
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```