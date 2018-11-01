
### webstorage

H5的本地存储,提供了本地存储方式，可以存储数据，分为localStorage/sessionStorage /indexdb/globalStorage。。

localStorage是永久保存，除非手动删除

sessionStorage是会话保存，会话结束，数据销毁

增：

localStorage.setItem(key, value)
localStorage[key] = value;

删：

localStorage.removeItem(key)

清空:

localStorage.clear();

改：

localStorage.setItem(key, value)
localStorage[key] = value;

查：

localStorage.getItem(key)
localStorage[key]

遍历:

localStorage.length

localStorage.keys(index) 得到键名


##### webStorage和cookie的区别

localStorage是永久保存，除非手动删除

sessionStorage是会话保存，会话结束，数据销毁


大小： cookie存储4KB，webStorage 存储5M

生命周期： cookie有过期时间，localStorage永久，session Storage会话

带宽： cookie会跟随请求头在前后端之间传递，影响带宽，可以用于前后端数据交互

限制： cookie可以设置path限制

操作性： cookie操作较难


#### 移动端事件/交互

在移动端中很多的事件都有变化，交互方式也与PC端大不项目，比如，在移动端中没有鼠标自然就没有鼠标事件

取而代之的是touch事件：

touchstart 手指触摸到后立即触发，相当于mousedown

touchmove  手指触摸之后进行移动时持续触发 相当于先mousedown再mousemove

touchend 手指离开屏幕立即触发，相当于mouseup

touchcancel 在系统取消触摸时触发

> 注意： 当我们使用ontouchXXX来绑定事件的时候，在移动端某些浏览器经常会失效，利用事件监听的方式addEventListener

事件对象与鼠标事件有一些区别：

无法直接从事件对象中得到距离相关的属性，有这样的几个属性供我们使用： tocuhes/targetTouches/changedTouches

这三个touches里存储几个手指的信息（clientX....）

> 注意： 在touchend事件中只能使用changedTouches

作业： 请实现移动端拖拽


##### 移动端click事件的300ms延迟

移动端浏览器中其实click是可以使用的（因为a标签这样的东东底层自带的就是click），但是使用的时候会有300ms的延迟（有点浏览器不会）,用户体验不好

###### 300ms延迟的来历

因为在移动端中，双击的时候两次点击间隔时间长，一开始的时候safari就做了这样的处理：点击一次后，等待一段时间，如果没有第二次点击，才算单击，所以就有了300ms的延迟，后来andriod就又模仿过来了，就成了一个规矩了。

一般会选择利用touch事件来取代click事件，所以会使用tap等相关事件（longtap/doubletap/swipe。。。。）这些事件都是自定义事件，需要利用touch事件来封装。

tap： 当元素触发了touchstart之后在很短的事件内又触发了touchend，并且移动的距离很小，并且没有触发touchmove，我们就认为这是一次tap

其实，我们可以使用[touch.js](https://www.cnblogs.com/Chen-XiaoJun/articles/5826698.html)/hammar.js这样的手势库来使用这些tap等事件

我们在移动端开发的时候其实基本上不会使用jquery，会使用jquery-mobile.js / zepto (推荐大家使用)

100k -> 50k -> 10k

script src = "zepto.min.js"

zepto.min.js.gzip


zepto把touch模块分离出去了，使用touch模块的时候需要引入.

##### 移动端的点透bug

我们在开发项目的时候可能会出现这样的情况：

点击上方的遮罩层的时候，想要让遮罩层消失，如果下方有一个a标签的时候，我们发现遮罩消失后，a标签跳转页面了。

上面的情况其实就已经触发了点透bug。

点透bug的实现场景：

1. 上层元素使用的是tap事件，并且tap之后消失

2. 下层元素使用的是click事件

此时，当我们tap上层元素消失后，下层元素的click事件会被触发。


解决办法:

1. 全部使用tap  缺点： 我们还得处理像a标签这样的东西

2. 使用缓动动画 让上层元素延迟消失 缺点： 与产品交互设计不符合

3. 加入中间层 在上层元素和下层元素中间加入一个透明的层，给这个中间层绑定click事件，并且click之后消失 缺点： 多操作一个dom

4. 使用fastclick库，可以解决click300ms延迟问题

