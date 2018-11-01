
### Node.js基础 一


##### 了解Node.js

Node.js® is a JavaScript runtime built on Chrome's V8 JavaScript engine.
> Node.js 是一个基于chrome的V8 引擎开发的js运行环境

所以 Node.js 是一个运行环境，可以运行JavaScript

Node.js 的常用作用:
	
    * Node.js作为后端存在，大部分情况下是作为中间服务器的，不会作为完全的后端开发选择
	* 可以开发一些npm包或者一些桌面端工具（vscode）
	* 区块链中大部分功能都是由Node.js和go实现的
    * 充当前端开发中的工程化承载 ...

Node.js优缺点以及适用的项目

    * 适合I/O密集型的应用，处理高并发较好，速度很快
    * 不适合CPU密集型的应用，计算能力弱，稳定性稍差

是Node选择了JS，Ryan Dahl 选择了Javascript作为Node的运行语言

> chrome浏览器拥有渲染引擎（webkit）和脚本引擎（V8）
> Node.js 其实之前有个版本叫IO.js，后面来变回来了



##### Node.js的能力

Node.js 可以解析JS代码（没有浏览器安全级别的限制）提供很多系统级别的API，如：

- 文件的读写
- 进程的管理
- 网络通信
- ……


##### Node.js中js的运行方式

1. REPL (Read-eval-print-loop)

    打开chrome浏览器的console控制台，可以执行js代码，Node.js中也拥有V8引擎，所以也可以直接再运行了Node的控制台中实现js代码

    cmd中执行 node 命令 可以进入Node的控制台

    注意，Node.js作为Js的宿主的时候是没有BOM,DOM的

> JS的对象分为三种：本地对象 非静态对象 能new Array，Object，内置对象 静态对象 不能new Math,global，宿主对象 BOM DOM 

  

2. 脚本模式
    
    可以利用 node filepath 这样的命令来直接执行脚本文件中的js代码

> 体验Node.js神奇的功能 fs，创建hello.txt, 写入 hello world，思考：这样安全吗?


##### Node.js 与模块化

Node.js中实现模块化使用的是CommonJS规范

Node.js中的模块可以大致分为三类：

    1. 内置模块, 安装Node.js的时候就已经存在了，不需要再下载和安装了

    2. 第三方模块 需要下载和安装 npmjs.com

>  内置模块和第三方模块的包都会放在node_modules中，这个目录中的模块不需要加路径，直接引入

	3. 自定义模块

	module.exports会被暴露出去，利用require方法导入模块的时候，返回值就是模块中module.exports的值（默认为{}）
	也可以利用exports来进行暴露，exports只是module。exports的引用, 所以不能再指向其他的对象，可以直接挂载属性用以暴露属性
	
	注意，进行模块化开发的时候，避免循环依赖
		


##### npm的使用入门

npm是Node.js自带的包管理器(Node.js package manager)

市场上还有很多常用的第三方的包管理器： cnpm , yarn , bower

npm init || -y  初始化Node.js包 -> package.json

npm list || [grep] package  查看项目中的依赖包的信息 || 只查看某个包的信息

npm info package  查看某个包的详细信息，例如版本...

npm install(@version) || uninstall package --save-dev...


> 版本号相关知识：每一个软件/工具/包都会存在自己的版本号，大部分情况下版本号由3个数组成（major, minor, build/reversion）.主版本号（1）：当功能模块有较大的变动，比如增加多个模块或者整体架构发生变化。此版本号由项目决定是否修改。子版本号,当功能有一定的增加或变化，比如增加了对权限控制、增加自定义视图等功能。此版本号由项目决定是否修改。阶段版本号：一般是 Bug 修复或是一些小的变动，要经常发布修订版，时间间隔不限，修复一个严重的bug即可发布一个修订版。此版本号由项目经理决定是否修改。

npm outdated  查看有没有包可以更新的

current 当前安装的版本   wanted 想要安装的版本  lasted 最新的版本 

package.json中版本号前不带任何符合，代表我们永远都只想要wanted 这个版本，更新所有包的时候就不会更新当前的这个包,  ^代表锁定major, ~代表锁定前两个版本major,minor，  *代表最高版本


npm update 更新包 更新成wanted版本，wanted版本就是除去符号限制外最高的版本

接下来研究如何发布一个包？

> underscore.js 一个JavaScript实用库，提供了一整套函数式编程的实用功能，但是没有扩展任何JavaScript内置对象。它是这个问题的答案：“如果我在一个空白的HTML页面前坐下， 并希望立即开始工作， 我需要什么？“...它弥补了部分jQuery没有实现的功能,同时又是Backbone.js必不可少的部分。

1. 将包开发好

2. 在npmjs.com上注册账号

3. 确保npm的源是本身的源 https://registry.npmjs.org

4. npm adduser

5. npm publish

###### npm registry

registry 是Node.js 下载包的地址，默认是https://registry.npmjs.org, 我们通常会将源切换成淘宝镜像源，在这里给大家介绍一个npm registry 控制工具 - nrm

我们需要全局安装nrm  -> npm install nrm -g

常用命令: nrm ls 查看当前源以及可用的源 ; nrm use <registry> 切换源 ; nrm test <registry> 测试全部源或者某个源的速度


###### nvm 可以让我们的电脑上同时存在多个node版本

1. 下载nvm

2. [安装nvm](https://www.jianshu.com/p/d0e0935b150a-hrl)

2.  常用命令：

	nvm ls 查看当前拥有的Node.js的版本

	nvm use version 切换Node.js版本

	nvm install version

	nvm uninstall version

##### Node.js的常用模块
	
Node.js中有很多常用的内置模块，例如Url，querystring，path，http，https....


###### URL

url 模块提供了一些实用函数，用于 URL 处理与解析

1. url.parse(urlString[, parseQueryString [, slashesDenoteHost] ])

> http协议的默认端口是80，https的默认端口是443

2. url.format(urlObject)

3. url.resolve(from, to)
       
###### QueryString

querystring 模块提供了一些实用函数，用于解析与格式化 URL 查询字符串。

1. querystring.parse(str[, sep[, eq[, options]]])

2. querystring.stringify(obj[, sep[, eq[, options]]])

3. querystring.escape(str)

4. querystring.unescape(str)


###### Http

使用http模块来创建使用 HTTP 服务器与客户端

1. 创建一个Web服务器（http.createServer）

> 开发好服务器或者各种api接口的时候，应该去进行测试，建议大家使用 postman, insomnia














  	 	
insomnia/postman   supervisor/hotnode/nodemon...
获得接口数据( http.get )
小爬虫(http.request 的get)
模拟表单提交(http.request 的post)

