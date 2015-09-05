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
##### 编译方式
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
  * gulp

###### 例子
```
// grunt
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


```
// gulp
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
##### 编译输出方式
* 嵌套输出方式 nested
```
sass --watch test.scss:test.css --style nested
```
* 展开输出方式 expanded
```
sass --watch test.scss:test.css --style expanded
```
* 紧凑输出方式 compact
```
sass --watch test.scss:test.css --style compact
```
* 压缩输出方式 compressed
```
sass --watch test.scss:test.css --style compressed
```

###### 例子
```
// sass
nav {
    ul {
        margin: 0;
        padding: 0;
        list-style: none;
    }
    
    li { display: inline-block; }
    
    a {
        display: block;
        padding: 6px 12px;
        text-decoration: none;
    }
}
```
```
// nested
nav ul {
    margin: 0;
    padding: 0;
    list-style: none; }
nav li {
  display: inline-block; }
nav a {
    display: block;
    padding: 6px 12px;
    text-decoration: none; }
```
```
// expanded
nav ul {
    margin: 0;
    padding: 0;
    list-style: none;
}
nav li {
    display: inline-block;
}
nav a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
}
```
```
// compact
nav ul { margin: 0; padding: 0; list-style: none; }
nav li { display: inline-block; }
nav a { display: block; padding: 6px 12px; text-decoration: none; }
```
```
// compressed
nav ul{margin:0;padding:0;list-style:none}nav li{display:inline-block}nav a{display:block;padding:6px 12px;text-decoration:none}
```
#### Sass调试
```
// 早期版本
sass --watch --scss --sourcemap style.scss:style.css
// Sass3.3 版本之上不需要添加 --sourcemap
sass --watch style.scss:style.css
```
![](sass debug.gif)
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
#### 声明变量
```
// 声明变量的符号“$”+变量名称:赋予变量的值;
$width: 200px;
// !default表示默认值
$color: #f00 !default;
// sass 的默认变量一般是用来设置默认值，然后根据需求来覆盖的，覆盖的方式也很简单，只需要在默认变量之前重新声明下变量即可。
$baseLineHeight: 2;
$baseLineHeight: 1.5 !default;
body{
    line-height: $baseLineHeight; 
}
```
#### 变量的调用
```
body {
    width: $width;
    color: $color;
    line-height: $baseLineHeight;
}
```
#### 变量的作用域
```
$color: orange !default;//定义全局变量(在选择器、函数、混合宏...的外面定义的变量为全局变量)
.block {
    color: $color;//调用全局变量
}
em {
    $color: red;//定义局部变量
    a {
        color: $color;//调用局部变量
    }
}
span {
    color: $color;//调用全局变量
}
```
#### 嵌套
* 选择器嵌套
```
nav {
    a {
        color: red;
    
        header & {
            color:green;
        }
    }  
}
```
* 属性嵌套
```
.box {
    border: {
        top: 1px solid red;
        bottom: 1px solid green;
    }
}
```
* 伪类嵌套
```
.clearfix{
    &:before,
    &:after {
        content:"";
        display: table;
    }
    &:after {
        clear:both;
        overflow: hidden;
    }
}
```
#### 混合宏
1、声明混合宏
```
// 不带参数混合宏
@mixin border-radius{
    -webkit-border-radius: 5px;
    border-radius: 5px;
}
// 带参数混合宏
@mixin border-radius($radius:5px){
    -webkit-border-radius: $radius;
    border-radius: $radius;
}
// 复杂的混合宏
@mixin box-shadow($shadow...) {
    @if length($shadow) >= 1 {
        @include prefixer(box-shadow, $shadow);
    } @else{
        $shadow:0 0 4px rgba(0,0,0,.3);
        @include prefixer(box-shadow, $shadow);
    }
}
```
2、调用混合宏
```
@mixin border-radius{
    -webkit-border-radius: 3px;
    border-radius: 3px;
}
button {
    @include border-radius;
}
```
3、混合宏参数
```
// 传一个不带值的参数
@mixin border-radius($radius){
    -webkit-border-radius: $radius;
    border-radius: $radius;
}
.box {
    @include border-radius(3px);
}
// 传一个带值的参数
@mixin border-radius($radius:3px){
    -webkit-border-radius: $radius;
    border-radius: $radius;
}
.btn {
    @include border-radius;
}
// 传多个参数
@mixin center($width,$height){
    width: $width;
    height: $height;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-top: -($height) / 2;
    margin-left: -($width) / 2;
}
.box-center {
    @include center(500px,300px);
}
// 特别的参数“…”
@mixin box-shadow($shadows...){
    @if length($shadows) >= 1 {
        -webkit-box-shadow: $shadows;
        box-shadow: $shadows;
    } @else {
        $shadows: 0 0 2px rgba(#000,.25);
        -webkit-box-shadow: $shadow;
        box-shadow: $shadow;
    }
}
.box {
    @include box-shadow(0 0 1px rgba(#000,.5),0 0 2px rgba(#000,.2));
}
```
#### 继承与扩展

```
.btn {
    border: 1px solid #ccc;
    padding: 6px 10px;
    font-size: 14px;
}

.btn-primary {
    background-color: #f36;
    color: #fff;
    @extend .btn;
}

.btn-second {
    background-color: orange;
    color: #fff;
    @extend .btn;
}
```
#### 占位符 %placeholder
```
%mt5 {
    margin-top: 5px;
}
%pt5{
    padding-top: 5px;
}

.btn {
    @extend %mt5;
    @extend %pt5;
}

.block {
    @extend %mt5;
    
    span {
        @extend %pt5;
    }
}
```
#### 混合宏 VS 继承 VS 占位符
![](sass include extend.png)
#### 插值#{}
```
$properties: (margin, padding);
@mixin set-value($side, $value) {
    @each $prop in $properties {
        #{$prop}-#{$side}: $value;
    }
}
.login-box {
    @include set-value(top, 14px);
}
```
```
@mixin generate-sizes($class, $small, $medium, $big) {
    .#{$class}-small { font-size: $small; }
    .#{$class}-medium { font-size: $medium; }
    .#{$class}-big { font-size: $big; }
}
@include generate-sizes("header-text", 12px, 20px, 40px);
```
```
%updated-status {
    margin-top: 20px;
    background: #F00;
}
.selected-status {
    font-weight: bold;
}
$flag: "status";
.navigation {
    @extend %updated-#{$flag};
    @extend .selected-#{$flag};
}
```
#### 注释
* 类似 CSS 的注释方式，使用"/* ”开头，结属使用 ”*/"
* 类似 JavaScript 的注释方式，使用"//"

两者区别，前者会在编译出来的 CSS 显示，后者在编译出来的 CSS 中不会显示
#### 数据类型
* 数字：如，1、2、13、10px；
* 字符串：有引号字符串或无引号字符串，如，"foo"、'bar'、baz；
* 颜色：如，blue、#04a3f9、rgba(255,0,0,0.5)；
* 布尔型：如，true、false；
* 空值：如，null；
* 值列表：用空格或者逗号分开，如，1.5em 1em 0 2em 、Helvetica, Arial, sans-serif。
  * nth函数（nth function） 可以直接访问值列表中的某一项；
  * join函数（join function） 可以将多个值列表连结在一起；
  * append函数（append function） 可以在值列表中添加值； 
  * @each规则（@each rule） 则能够给值列表中的每个项目添加样式。

#### 运算
```
// 加法
.box {
    width: 20px + 8in;
}
// 减法
$full-width: 960px;
$sidebar-width: 200px;

.content {
    width: $full-width -  $sidebar-width;
}
// 乘法
.box {
    width:10px * 2;  
}
// 除法
// 如果数值或它的任意部分是存储在一个变量中或是函数的返回值。
// 如果数值被圆括号包围。
// 如果数值是另一个数学表达式的一部分。
// 如果两个值带有相同的单位值时，除法运算之后会得到一个不带单位的数值。
.box {
    width: (100px / 2);  
}
// 颜色运算
p {
    color: #010203 + #040506;
}
// 字符运算
$content: "Hello" + "" + "Sass!";
.box:before {
  content: " #{$content} ";
}

```
### Sass的控制命令
#### @if
```
@mixin blockOrHidden($boolean:true) {
  @if $boolean {
      @debug "$boolean is #{$boolean}";
      display: block;
    }
  @else {
      @debug "$boolean is #{$boolean}";
      display: none;
    }
}

.block {
  @include blockOrHidden;
}

.hidden{
  @include blockOrHidden(false);
}
```
#### @for
```
// $i:表示变量  start:表示起始值  end:表示结束值
// 关键字 through 表示包括 end 这个数，而 to 则不包括 end 这个数。
@for $i from <start> through <end>
@for $i from <start> to <end>
```
```
$grid-prefix: span !default;
$grid-width: 60px !default;
$grid-gutter: 20px !default;

%grid {
  float: left;
  margin-left: $grid-gutter / 2;
  margin-right: $grid-gutter / 2;
}
@for $i from 1 through 12 {
  .#{$grid-prefix}#{$i}{
    width: $grid-width * $i + $grid-gutter * ($i - 1);
    @extend %grid;
  }  
}
```
#### @while
```
$types: 4;
$type-width: 20px;

@while $types > 0 {
    .while-#{$types} {
        width: $type-width + $types;
    }
    $types: $types - 1;
}
```
#### @each
```
@each $var in <list>
```
```
$list: adam john wynn mason kuroir;//$list 就是一个列表

@mixin author-images {
    @each $author in $list {
        .photo-#{$author} {
            background: url("/images/avatars/#{$author}.png") no-repeat;
        }
    }
}

.author-bio {
    @include author-images;
}
```
### Sass的函数
#### 字符串函数
##### unquote() & quote()
```
// unquote($string)：删除字符串中的引号
// unquote( ) 函数只能删除字符串最前和最后的引号（双引号或单引号），而无法删除字符串中间的引号。如果字符没有带引号，返回的将是字符串本身。
.test1 {
    content:  unquote('Hello Sass!') ;
}
// quote($string)：给字符串添加引号
.test1 {
    content:  quote('Hello Sass!');
}
// 使用 quote() 函数只能给字符串增加双引号，而且字符串中间有单引号或者空格时，需要用单引号或双引号括起，否则编译的时候将会报错。解决方案就是去掉空格，或者加上引号。
// 同时 quote() 碰到特殊符号，比如： !、?、> 等，除中折号 - 和 下划线_ 都需要使用双引号括起，否则编译器在进行编译的时候同样会报错。
```
##### To-upper-case() & To-lower-case()
```
// To-upper-case() 函数将字符串小写字母转换成大写字母
.test {
  text: to-upper-case(aaaaa);
  text: to-upper-case(aA-aAAA-aaa);
}
// To-lower-case() 函数将字符串转换成小写字母
.test {
  text: to-lower-case(AAAAA);
  text: to-lower-case(aA-aAAA-aaa);
}
```
#### 数字函数
##### percentage()
```
// percentage($value)：将一个不带单位的数转换成百分比值
// 如果您转换的值是一个带有单位的值，那么在编译的时候会报错误信息
.footer{
    width : percentage(.2)
}
```
##### round()
```
// round() 函数可以将一个数四舍五入为一个最接近的整数
.footer {
   width:round(12.3px)
}
```
##### ceil()
```
// ceil() 函数将一个数转换成最接近于自己的整数，会将一个大于自身的任何小数转换成大于本身 1 的整数。也就是只做入，不做舍的计算。
.footer {
   width:ceil(12.3px);
}
```
##### floor()
```
// floor() 函数刚好与 ceil() 函数功能相反，其主要将一个数去除其小数部分，并且不做任何的进位。也就是只做舍，不做入的计算。
.footer {
   width:floor(12.3px);
}
```
##### abs()
```
// abs( ) 函数会返回一个数的绝对值。
.footer {
   width:abs(-12.3px);
}
```
##### min() & max()
```
// min() 函数功能主要是在多个数之中找到最小的一个，这个函数可以设置任意多个参数
// 在 min() 函数中同时出现两种不同类型的单位，将会报错误信息
>> min(1em,2em,6em)
1em
// max() 函数和 min() 函数一样，不同的是，max() 函数用来获取一系列数中的最大那个值
>> max(1px,5px)
5px
```
##### random()
```
// random() 函数是用来获取一个随机数
>> random()
```
#### 列表函数
##### length()
```
// ength() 函数主要用来返回一个列表中有几个值，简单点说就是返回列表清单中有多少个值
// length() 函数中的列表参数之间使用空格隔开，不能使用逗号，否则函数将会出错
>> length(10px 20px (border 1px solid) 2em)
4
```
##### nth()
```
nth($list,$n)
// nth() 函数用来指定列表中某个位置的值。不过在 Sass 中，nth() 函数和其他语言不同，1 是指列表中的第一个标签值，2 是指列给中的第二个标签值，依此类推。
>> nth(10px 20px 30px,1)
10px
```
#### 颜色函数
#### Introspection 函数
#### 三元函数
#### 自定义函数
```
