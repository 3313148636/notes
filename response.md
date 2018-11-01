### 响应式布局

我们有些项目，运行的终端需要PC和Mobile ，也就说，用户会从pc端访问，也会从移动端来访问项目

一般处理情况如下:

* 我们会针对一些设备专门开发一个应用版本，这样的话开发成本就会较高。

* 我们可以只开发一套网站，然后利用响应式技术，让页面中的内容/结构根据屏幕大小来进行调整。

响应式的原理，在一套网页中同时开发好PC的内容和移动的内容，监听设备的屏幕大小，控制内容显示

像京东这样的项目，页面结构/业务逻辑非常的庞大，不适合响应式开发

像企业站/宣传页之类的内容结构叫啥，交互较少的项目适合用响应式

#### 响应式的实现方式

响应式主要是依赖css来进行处理，需要处理的有:

1. 根据屏幕控制一些元素的显示和隐藏

2. 根据屏幕控制页面元素的排列方式

响应式的实现，主要有两种：

实现响应式的时候，需要将屏幕进行划分，一般情况下分为三种情况： PC/PAD/Mobile

设置两个阀值来将屏幕范围分割成上述的三种屏幕： 768 / 992

实现响应式主要依靠的CSS3的媒体查询 @media

> 注意，响应式一般考虑的只有宽度，不考虑高度

1. 定义好各个屏幕的样式文件，然后利用媒体查询控制引入不同的样式文件是否生效

<link rel="stylesheet" type="text/css" media="screen and (max-width:960px)" href="style.css">

screen 代表彩色屏幕 其他的还有很多种，大部分情况下只使用screen和all（代表所有屏幕类型）

and 后面是 判断条件

这种方法一般不会使用，因为这样的话页面会同时加载多个css文件，http请求次数增多，而且一般实现响应式的项目内容结构不多，需要做处理的地方也不多，完全可以把css都写在一个css文件中，利用下面这种方法去实现

2. 在css中利用@media关键帧来定义响应样式

@media screen and (min-width: 992px) {
    // 这个里面的样式会在屏幕符合条件的情况下拿出来
}

响应式代码编写中有一个原则： 小屏优先，也就是说在编写样式的时候先写小屏样式, 这样的话，在移动设备中，渲染效率较高，对移动设备较为友好。

/* mobile */
body { background: deeppink; }


@media screen and (min-width: 768px) {
    body { background: blue; }
}


@media screen and (min-width: 992px) {
    body { background: red; }
}

目前前端的布局方式主要有这么几种:

固定布局 （pc）, 流式布局（自适应布局，.. 利用一些自适应单位来进行布局 % .. ） , 响应式布局 （固定布局和自适应布局加上媒体查询）

#### Bootstrap里的十二栅格化实现响应式 

bootstrap是一个很流行的前端框架（html， css， javscript）, 最知名的是内部用来实现响应式开发的栅格系统（十二栅格化/十二网格化/网格系统...）

bootstrap定义屏幕为4种，3个阈值 xs  768  sm  992  md  1200  lg

boot 定义了两种容器 container container-fluid (100%)

container的宽度 在xs中，宽度是 100%， 768-992  750， 992-1200 970， 1200- 1170

boot将容器内部利用 row 类名讲容器分成了12份, 利用col-type-num 类名就可以规定row中的某个元素在type类型的屏幕中占12份中的num份

col-type-offset-num 用来控制元素做偏移 

visible-type; hidden-type 控制元素在什么屏幕下显示或者在什么屏幕下隐藏


> bootstrap可以根据需求定制，除了bootstrap还有很多的前端框架供我们选择，例如pure.css就是一个更小更纯净的响应式前端框架...

#### 制作一个响应式框架 

1. 搭建gulp开发环境

> 安装包的时候可以在后面跟一些cans, -g / --global代表着全局安装，--save / -S 代表着生产依赖（打包到项目中）, --save-dev / -D 代表着这是开发依赖 (只是在开发的时候使用的工具)，无论是生产依赖还是开发依赖都会设置在package.json中.

2. 使用sass开发

    sass基于ruby的，其实理论上来说，最好使用ruby开编译sass，也可以用node.js来编译

    [sass 笔记](http://note.youdao.com/noteshare?id=8227b3b214b3645924ffbc5dfaa40b15)

> css reset 指的就是样式重置，为了保证跨浏览器一致性，有一个normalize.css，专门做css reset















