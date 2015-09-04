# Sass
#### Mac安装
```
//前提安装了ruby，检测
ruby －v
//gem仓库站点 https://rubygems.org
sudo gem install sass
//如果被墙，可直接下载后再安装
sudo gem install path/sass-3.14.18.gem
//检查安装版本
sass -v
```
#### 编译
```
//sass
sass --watch test.sass:test.css
//scss
sass --watch test.scss:test.css
```
#### 更新
```
gem update sass
```
#### 卸载
```
gem uninstall sass
```
## Sass与Scss的区别
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