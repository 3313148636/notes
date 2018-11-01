### HTML5-CSS3 第二部分

CSS3新增的属性有很多，其中最常用/最好用的有 animation/transition/transform (2d, 3d)...

#### Transition 过渡

在元素的css属性变化的过程中，加入逐渐变化过渡效果

transition 子属性有：

transition-property 确定需要加过渡的属性 属性可以空格隔开写多个，all （代表所有属性）

transition-duration 过渡时长 单位为s , 例如： 2s

transition-timing-function 过渡时间曲线

    linear	规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。
    ease	规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）。
    ease-in	规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。
    ease-out	规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。
    ease-in-out	规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。
    cubic-bezier(n,n,n,n)	在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。[在线工具](http://cubic-bezier.com)

transition-delay 延迟  单位为s


> 浏览器对CSS3动效的渲染是很高效，所以在移动端中将会大量使用css动效。

> CSS3属性是有一定的兼容问题，IE低版本浏览器肯定是不行，其他现代浏览器的低级版本可能需要加各种各样的前缀，在这里介绍几个工具，gulp-autoprefixer,post-css, [在线编译工具](http://autoprefixer.github.io/)


#### Transform 转换/变形  

CSS3提供的变形场景有两种：2d和3d

提供的变形效果： translate (位移) , scale (缩放) , rotate (旋转) , skew (拉伸)

> 变形效果类似于相对定位，不会影响文档流，不会造成回流。
> 变形后，内容节点都会伴随着元素变形。

transform : translate(x, y)...
transform : scale(x, y)...
transform : rotate(deg)... // deg代表角度， 默认是顺时针转动, 中心点为几何中心
transform : skew()...



transform-origin 可以控制变形参考的原点


在3D转换中，其实就是多了一个坐标轴Z

想要显示3d效果，元素需要一个舞台,然后添加一个perspective（摄影机）属性, 做3d转换的元素，需要加transform-style: preserve-3d;

我们看到的效果是摄像机拍摄下来的效果，通过perspective-origin调整摄影机的位置


旋转之后，坐标系是会变化的

translate:

    translateX,translateY,translateZ,translate3d(x, y ,z)

rotate: 

    rotateX.....

scale: 

    scaleX...


### Animation

在使用css3 里animation的时候，需要去定义一个动画

利用 @keyframes 关键字来定义动画
```
@keyframes name {
    // 在这里写动画执行的过程中各个阶段的css样式
    // 可以使用%和from/to来定义某个阶段的样式
}
```

```
 /* from (0%) to (100%) */
@keyframes ani-circle {
    from { transform: translate(0, 0); }
    25% { transform: translate(1000px, 0); }
    50% { transform: translate(1000px, 500px); }
    75% { transform: translate(0, 500px); }         
    to { transform: translate(0, 0) }
}

```

定义好动画后，我们通过为元素添加animation属性来使用动画

animation-name 指定使用的动画的名字

animation-duration 指定动画执行的时间

animation-timing-function 时间曲线 

animation-delay 动画延迟时间

animation-iteration-count 动画执行的次数 n  infinite ( 无限次 )


animation-direction  指定动画的运行方向
    normal	默认值。动画按正常播放。	测试 »
    reverse	动画反向播放。	测试 »
    alternate	动画在奇数次（1、3、5...）正向播放，在偶数次（2、4、6...）反向播放。	测试 »
    alternate-reverse	动画在奇数次（1、3、5...）反向播放，在偶数次（2、4、6...）正向播放。	测试 »
    initial	设置该属性为它的默认值。请参阅 initial。	 
    inherit	从父元素继承该属性。请参阅 inherit。


animation-play-state 控制动画暂停 paused running

animation-fill-mode: forwards; 控制动画执行完成后元素停留在动画结束的位置



> 注意：在某些情况下，因为浏览器执行和编辑js的时候因为效率和性能问题，会导致一些效果不能实现，这个时候可以使用setTimeout(()=>{},0)将目标代码放入到异步队列中，分开执行，效果就会出现。

> 在日常工作中，我们会使用到animate.css动画库，它里面封装好了绝大部分的常用动画。


