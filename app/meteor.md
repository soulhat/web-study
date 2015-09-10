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
microscope
    -microscope.css
    -microscope.html
    -microscope.js
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

