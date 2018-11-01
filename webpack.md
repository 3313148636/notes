
### 前后台项目开发

前端项目：

前段时间时间开发了拉钩的前台项目（直接给用户使用），还需要开发一个后台项目（CMS管理系统）

后端项目：

整个拉钩后端的服务器


### 搭建前端的CMS项目开发环境

选用webpack作为工程化工具，webpack是一个基于配置的前端模块化打包工具。

需要先去下载webpack  

全局安装了webpack，执行命令的时候就可以直接webpack来执行了，如果全局没有装，只在本地装包，只能执行./node_modules/bin/webpack ...

为了使用更方便，一般会全局安装，webpack目前版本为4.X，需要搭配webpack-cli使用

在本地也需要安装 webpack

#### webpack的使用

可以直接通过webpack命令来执行打包操作，通过 --env来配置一些参数

webpack的基本命令：

--help 查看所有的命令

--mode development production none

-o 配置出口

--config 配置 配置文件

一般都是通过配置文件来进行使用：

webpack默认会根据webpack.config.js中的配置进行模块化打包


#### 插件Plugin

plugin是解决loader做不了的事情的，现在准备让html页面出现在输出目录中

使用 html-webpack-plugin

插件和入口出口一样需要直接在配置文件中进行配置,在其中放入插件的实例就相当于在使用插件

copy-webpack-plugin 可以将文件进行复制

#### dev server

我们使用webpack-dev-server工具来替代webpack，在启动的时候就会开启热更新服务

> 其实工程化工具不止gulp/webpack/grunt，还有很多，比如parcel， rollup


### css/scss/...等文件的处理

在开发的时候可以在js文件中引入css/scss文件，但是我们需要使用loader来处理

css-loader 可以将引入到js中的css模块中的代码放入到js中

style-loader 可以将js中的css代码放入到style标签中去

sass-loader  可以将sass代码编译成css代码

针对图片，我们直接将图片打包（复制）到输出目录，直接引入，也可以模块化使用

url-loader 基于file-loader，专业处理图片，但是在文件大小（单位 byte）低于指定的限制时，可以返回一个 DataURL。

babel-loader 编译ES高级语法

####  使用前端框架来快速搭建项目结构
​
使用基于bootstrap的ui框架 adminLTE 来快速开发CMS应用


​



​	




















