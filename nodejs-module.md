###### Http

使用http模块来创建使用 HTTP 服务器与客户端

1. 创建一个Web服务器（http.createServer）

> 开发好服务器或者各种api接口的时候，应该去进行测试，建议大家使用 postman, insomnia

服务器应该能接收请求，响应请求

服务器应该分为两种服务器：webserver，api server

webserver 负责接受全部的请求，然后对请求进行分流，交给apiserver处理之后，webserve将数据返回给客户端

api server 负责主要业务逻辑，提供api接口

但是如果是中小级企业项目的话一般不需要部署两种服务器，而是把所有的功能都开发在一台服务器中 （资源请求， api请求）

一个服务器主机可以开启多个服务器，域名/ip对应的都是服务器主机，如果想进入到服务器主机中开启的某一个服务器的话，需要在域后面跟上port 

> 介绍一种小工具，可以帮助我们热启动脚本，例如:supervisor, hotnode, nodemon

createServer(requestListner) 可以创建服务器
server.listen（prot， hostname， callback）

requestListner = (request, response) => {
    request: url, method, headers
    response: write, end, writeHead
}


2. http.get 接口数据 

Node.js可以作为中间服务器存在，所以Node.js必须可以与其他的服务器进行交互（没有跨域问题）

$.ajax || http.request  $.get || http.get 

注意，数据接受的时候： 第一要去判断数据是否获取成功，第二:获取到数据的时候需要利用data/end事件来接收最终的数据


3. 爬虫小案例

    SEO（搜索引擎优化）是评价网站的重要因素，影响网页在搜索结果中的排位，搜索引擎是根据爬虫爬取到的内容进行评判SEO优先级的 

    爬虫爬取的是什么呢？

    爬取的HTML文件，爬取的是未被浏览器加载/渲染的源文件

    利用http模块请求到html文件内容后，可以利用cheerio对其进行解析处理，然后就可以像使用jq一样去操作html内容中的目标元素了


4.  模拟表单提交 post

    注意，post过来的数据，我们在Node中接收的方式是：

    let _body = ''
    req.on('data', (chunk) => {
        _body += chunk
    })
    req.on('end', (chunk) => {
        console.log(_body)
    })

> API接口其实就是服务器对解析后的请求url路径和业务逻辑进行的映射

### Events

Node.js提供了Events模块帮我们进行事件的定义/绑定/触发

其实在Web开发中也存在自定义事件的能力

```
var event = new Event('build');

// Listen for the event.
window.addEventListener('build', function(e) { ... }, false);

// Dispatch the event.
window.dispatchEvent(event);
```

JQuery也有封装好的，兼容处理好的自定义事件机制

```
$(window).on('ownmaker', () => {
    console.log('jaja')
})

$(window).trigger('ownmaker')
```

其实EventEmitter的原型上存在绑定/触发等自定义事件相关api，我们可以借助这些方法来进行自定义事件的操作

其实他可以用于发布订阅的实现，其中，订阅利用绑定事件来完成，发布利用触发事件来完成




### File System


1. 创建一个目录：mkdir

> 文件操作API一般都有同异步两种方法，例如 mkdir, mkdirSync, 同步方法慎用，一旦出错，就会抛出错误, 使用异步方法一般回调函数都会接受错误信息，可以根据错误信息做出处理，不一定要阻塞下面代码运行

2. 创建文件并写入内容：writeFile , appendFile

3. 读取文件的内容：readFile  

> Node.js中回调函数会优先返回错误信息，错误前置

4. 删除文件：unlink, rmdir(无法删除非空文件夹)...

5. 重命名目录或文件：rename

6. 批量创建文件

7. 列出目录的东西：readdir  得到文件与目录的信息：stat  isFile()

8. 监听文件改变 watch  watchFile ... 

遇到的问题：

* 删除目录的时候不能删除非空目录，所以需要一层一层遍历，利用递归将目录里面所有的东西都删掉之后才能删除目录，麻烦

* fs有很多异步方法，但是都没有支持Promise

9. fs-extra

    既扩展了fs模块的常用方法，而且支持了Promise

> 回调地狱： Node.js中存在大量的异步操作，而这些异步操作都有联系，容易形成回调函数多层嵌套的情况，这样的代码质量不高，我们成为回调地狱

> 使用了node-xlsx模块完成了excel表格读取....


### Stream 文件流

fs提供了文件读取流和文件写入流两种文件流，可以利用他们来操作文件，减少对内存的占用。。。

createReadStream 创建读取流
createWriteStream 创建写入流

readStream.on('data||end')
writeStream.write('chunk')



### Zlib

zepto官网说，自己的文件压缩后是9.6k，我们发现下载之后的zepto.min.js 有20多k，这是怎么回事儿？

9.6k是进行gzip压缩之后的大小，一般进行文件压缩，先对内容进行压缩... (50- > 20) ,还可以再用gzip的方式进行进一步压缩 （20k -> 9.6）

Node.js提供了zlib模块供我们进行压缩等操作

gzip压缩后，生产的gz文件可以直接引入到页面中，但是此时浏览器很可能不识别

我们需要的就是让浏览器请求回来gz文件后，自己解压，所以需要再响应头中写入：

'Content-Encoding': 'gzip'


### 异步操作

1. promise-async-await

   * promise 管控异步动作， 可以通知异步队列中多个promise执行顺序

    串行：链式调用  并行 Promise.all......

    * async await 管控异步队列

    需要一个async 函数， 再async函数中对异步操作进行await 下面的代码就会等到异步动作执行完成后再执行，并且异步动作的返回值就是promise的resolve传入的值

    记住，想要async和await，就必须得有promise



3. async  可以帮助我们管理异步进程



   串行无关联  series

   并行无关联 parallel

   瀑布流 串行有关联 waterfall



### 简易Router



