

### 环境中将 es6 代码编译成 es5

babel 是一个专业编译js的工具 例如 es6/7/8 -> ES5; typescript => javascript ; coffeescript => javascript ....

如果是再gulp里使用babel，可能需要装gulp-babel , 我们再项目中对js管理使用的是webpack，所以需要安装webpack中的babel工具： babel-loader

> webpack中编译各种文件/各种代码用的工具叫loader...

npm install -D babel-loader @babel/core @babel/preset-env

光装上面的不够，因为我们还使用了更高级js语法，所以还得装

npm install @babel/runtime

npm install -D @babel/plugin-transform-runtime

还有问题，原因是因为 代码中出现了commondJS规范，模块化也必须使用ES6 modules。


### 使用Better-scroll来实现首页的区域滚动与上下拉刷新

我们再开发中经常会使用一些区域滚动插件来优化我们的页面滚动效果（加一些反弹效果。。。）

这样的插件我们以前用的最多的是Iscroll，现在用的最多的是better-scroll

具体使用请查看[文档](https://ustbhuangyi.github.io/better-scroll/doc/zh-hans/#%E8%B5%B7%E6%AD%A5)

1. 注意，better-scroll的容器中只有第一个子元素才能进行滚动

2. 保证better-scroll的容器的尺寸是小于里面内容的长度的

3. 当容器内内容区域高度变化之后，一定要让bscroll重新计算


###  配置生产环境和开发环境

我们需要对我们的环境再进行配置，完成生成环境的构建

配置两个gulpfile文件，配置dev和build命令（npm scripts） 

gulp --gulpfile ./gulpfile-dev.js // 可以执行gulp运行的配置文件

开始再gulpfile-build进行生产配置

1. 去掉不必要的任务

2. 为代码添加版本号

    因为浏览器有缓存机制会缓存一些外链文件....,但是缓存机制有时又一个缺点，当服务器上的代码更新之后，用户访问到的却依然是缓存中的上一个版本的代码

    浏览器缓存依靠的是文件的路径，也就说我们版本迭代之后将资源文件引入的路径修改之后，浏览器就会访问最新的版本代码

    所以我们会使用gulp-rev工具帮助我们为资源文件添加路径，并且通过rev.manifest来创建写有资源映射关系的json文件

    再利用 gulp-rev-collector 为html页面替换最终的带有版本号的资源路径

    但是出现了问题，因为default中任务是并联执行的，所以当html输出的时候，资源文件还没有打包出来，所以引入失败

    使用了gulp-sequence 来调整任务执行顺序。



### 架构

系统结构

    B/S （网站）   C/S

    RMVC   ( Router , model, view, controller )  SPA    


Router

    自定义开发项目专属路由工具

数据渲染模式

    前后端分离架构  

    数据的渲染方式： ssr / bsr

    ssr  缺点： 不好维护  优点： 对SEO友好

    bsr 


UI渲染  

    模板引擎  handlebars



开发环境    

    gulp + webpack   ， 开发环境和生产环境

    ........     ........ 











