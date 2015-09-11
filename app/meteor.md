## Meteor
### meteor 安装
```
$ curl https://install.meteor.com | sh
```
#### 案例1.1
```
$ meteor create microscope
$ cd microscope
$ meteor
```
访问 http://localhost:3000
看到如下网页
![](http://7xlot8.com1.z0.glb.clouddn.com/2-1.png)
microscope目录结构如下：
```
|-- microscope
    |-- microscope.css
    |-- microscope.html
    |-- microscope.js
```
### 添加代码包
```
meteor add twbs:bootstrap
meteor add underscore
```
与“传统”方式添加外部资源不同，我们不需要在代码中链接任何 CSS 或 JavaScript 文件，因为 Meteor 已经帮我们搞定了！这就是 Meteor 代码包的众多优势之一。

添加Bootstrap包后，查看现有应用的变化如下：
![](http://7xlot8.com1.z0.glb.clouddn.com/2-1b.png)
#### 关于代码包

Meteor 中的代码包有点特殊，分为五种：

* Meteor核心代码本身分成多个核心代码包（core package），每个 Meteor 应用中都包含，你基本上不需要花费精力来维护它们。
* 常规Meteor代码包称为“isopack”，或同构代码包（isomorphic package，意味着它们既能在客户端也能在服务器端工作）。第一类代码包例如accounts-ui或appcache由Meteor核心团队维护，与Meteor捆绑在一起。
* 第三方代码包就是其他用户开发的isopack上传到Meteor 的代码包服务器上。你可以访问Atmosphere或meteor search 命令来浏览这些代码包。
* 本地代码包（local package）是自己开发的代码包，保存在 /packages 文件夹中。
* NPM 代码包（NPM package）是Node.js的代码包，虽不能直接用于 Meteor，但可以在上述几种代码包中使用。

### 部署
#### 部署在 Meteor
首先最简单的是部署到 Meteor 的子域名上（例如： http://myapp.meteor.com ），这是我们首先要去学习的。在项目早期，这对于展示你的应用和快速设置一个测试服务器都很有用途。

而部署在 Meteor 是非常简单的。打开终端，定位到你 Meteor 应用的目录，并输入：
```
meteor deploy myapp.meteor.com
```
要把“myapp”替换成你想要的名称，最好是命名一个没有被使用的。如果你的名称已经被使用，Meteor 会提示你去输入密码。如果发生这样的情况，只需通过 ctrl+c 来取消当前操作，然后用另一个不同的名称再试一次。

如果顺利地部署成功了，几秒钟后你就能够在 http://myapp.meteor.com 上访问到你的应用了。
#### 部署在 Modulus
[文档链接](部署在 Modulus)
#### Meteor Up[github](https://github.com/arunoda/meteor-up)
Meteor Up （简称 mup ）是另一个通过命令行的操作去帮助你解决安装和部署问题。所以让我们看看如何通过 Meteor Up 来部署 Microscope。
##### Meteor Up 的初始化
```
npm install -g mup 
// npm安装Meteor Up
```
创建一个单独的目录，为Meteor Up 提供一个特定的部署环境。我们使用单独的目录出于两个原因：
1. 这可以很好的避免里面包含任何你Git 存储库的隐藏文件，尤其如果你是在公共代码库去操作。
2. 通过使用多个单独的目录，能够并行地进行多个Meteor Up 管理和配置。这将会用在实际产品的部署以及分段实例的部署。

```
mkdir ~/microscope-deploy
cd ~/microscope-deploy
mup init
```
##### Meteor Up 的配置
当初始化一个新项目的时候，Meteor Up 会为了创建两个文件：mup.json和 settings.json。

mup.json会保存所有我们部署的相关设置，而settings.json 会保存所有应用的相关设置（OAuth token、Analytics token，等等）。

配置 mup.json 文件
```
{
  // 服务器身份验证
  "servers": [{
    "host": "hostname",
    "username": "root",
    "password": "password"
    //or pem file (ssh based authentication)
    //"pem": "~/.ssh/id_rsa"
  }],

  // MongoDB 配置
  "setupMongo": true,

  // Meteor 应用路径
  "app": "/path/to/the/app",

  // 环境变量
  "env": {
    "ROOT_URL": "http://supersite.com"
    // MAIL_URL
    // MONGO_URL
  }
}
```
##### 设置和部署
```
mup setup
// 设置服务器为 Meteor 应用托管
mup deploy
// 部署应用
```
##### 显示日志信息
```
mup logs -f
```
### Meteor 应用的文件结构
为了保持项目简洁，删除当前文件下的microscope.html、microscope.js 和 microscope.css。新建/client，/server，/public 和 /lib四个文件夹。结构如下：
```
|-- microscope
    |-- client 
    // client中的代码只会在客户端运行
    |-- server 
    // server中的代码只会在服务器端运行
    |-- public 
    // 将所有的静态文件（字体，图片等）放置在public中
    |-- lib
    // 其它代码则将同时运行于服务器端和客户端上
```
#### Meteor 加载文件顺序
1. 在 /lib 文件夹中的文件将被优先载入。
2. 所有以 main.* 命名的文件将在其他文件载入后载入。
3. 其他文件以文件名的字母顺序载入。

#### 命名法建议
* **javascript**使用驼峰命名法，如：myVariable
* **文件**使用下划线命名法，如：my_file.js
* **css类**使用连字号，如：.my-class

#### css
CSS 文件将被 Meteor 自动加载并简化。因此，不同于其它的静态文件都被放置于 /public 文件夹，请将 CSS 文件放入 /client 文件夹。请创建一个 client/stylesheets/ 文件夹并将以下 style.css 文件放置入内。
```
// style.css
.grid-block, .main, .post, .comments li, .comment-form {
    background: #fff;
    border-radius: 3px;
    padding: 10px;
    margin-bottom: 10px;
    -webkit-box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15);
    -moz-box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15);
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15); 
}

body {
    background: #eee;
    color: #666666; 
}

#main {
    position: relative;
}
.page {
    position: absolute;
    top: 0px;
    width: 100%;
}

.navbar {
    margin-bottom: 10px; 
}
/* line 32, ../sass/style.scss */
.navbar .navbar-inner {
    border-radius: 0px 0px 3px 3px; 
}

#spinner {
    height: 300px; 
}

.post {
    /* For modern browsers */
    /* For IE 6/7 (trigger hasLayout) */
    *zoom: 1;
    position: relative;
    opacity: 1;
}
.post:before, .post:after {
    content: "";
    display: table;
}
  .post:after {
    clear: both; 
}
  .post.invisible {
    opacity: 0; 
}
.post.instant {
    -webkit-transition: none;
    -moz-transition: none;
    -o-transition: none;
    transition: none; 
}
.post.animate{
    -webkit-transition: all 300ms 0ms;
    -moz-transition: all 300ms 0ms ease-in;
    -o-transition: all 300ms 0ms ease-in;
    transition: all 300ms 0ms ease-in; 
}
.post .upvote {
    display: block;
    margin: 7px 12px 0 0;
    float: left;
}
.post .post-content {
    float: left; 
}
.post .post-content h3 {
    margin: 0;
    line-height: 1.4;
    font-size: 18px; 
}
.post .post-content h3 a {
    display: inline-block;
    margin-right: 5px; 
}
.post .post-content h3 span {
    font-weight: normal;
    font-size: 14px;
    display: inline-block;
    color: #aaaaaa; 
}
.post .post-content p {
    margin: 0; 
}
.post .discuss {
    display: block;
    float: right;
    margin-top: 7px; 
}

.comments {
    list-style-type: none;
    margin: 0; 
}
.comments li h4 {
    font-size: 16px;
    margin: 0; 
}
.comments li h4 .date {
    font-size: 12px;
    font-weight: normal; 
}
.comments li h4 a {
    font-size: 12px; 
}
.comments li p:last-child {
    margin-bottom: 0; 
}

.dropdown-menu span {
    display: block;
    padding: 3px 20px;
    clear: both;
    line-height: 20px;
    color: #bbb;
    white-space: nowrap; 
}

.load-more {
    display: block;
    border-radius: 3px;
    background: rgba(0, 0, 0, 0.05);
    text-align: center;
    height: 60px;
    line-height: 60px;
    margin-bottom: 10px; 
}
.load-more:hover {
    text-decoration: none;
    background: rgba(0, 0, 0, 0.1); 
}

.posts .spinner-container{
    position: relative;
    height: 100px;
}

.jumbotron{
    text-align: center;
}
.jumbotron h2{
    font-size: 60px;
    font-weight: 100;
}

@-webkit-keyframes fadeOut {
    0% {opacity: 0;}
    10% {opacity: 1;}
    90% {opacity: 1;}
    100% {opacity: 0;}
}

@keyframes fadeOut {
    0% {opacity: 0;}
    10% {opacity: 1;}
    90% {opacity: 1;}
    100% {opacity: 0;}
}

.errors{
    position: fixed;
    z-index: 10000;
    padding: 10px;
    top: 0px;
    left: 0px;
    right: 0px;
    bottom: 0px;
    pointer-events: none;
}
.alert {
    animation: fadeOut 2700ms ease-in 0s 1 forwards;
    -webkit-animation: fadeOut 2700ms ease-in 0s 1 forwards;
    -moz-animation: fadeOut 2700ms ease-in 0s 1 forwards;
    width: 250px;
    float: right;
    clear: both;
    margin-bottom: 5px;
    pointer-events: auto;
}
```
```
// 目前目录结构
microscope
    |-- client 
        |-- stylesheets
            |-- style.css
    // client中的代码只会在客户端运行
    |-- server 
    // server中的代码只会在服务器端运行
    |-- public 
    // 将所有的静态文件（字体，图片等）放置在public中
    |-- lib
    // 其它代码则将同时运行于服务器端和客户端上
```
