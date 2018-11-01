### canvas

>是HTML5新增的标签（画布）,功能特别强大

作用：

1. HTML5游戏大部分都是基于canvas开发的，还有一些交互性强的效果 

2. 炫酷的动效

3. webGL技术的实现（利用canvas在页面中绘制3D效果），three.js，计算机图形学

4. 图片裁剪、修图、转base64

5. 绘制图表做数据可视化(echarts highcharts d3.js)

### 如何使用canvas

canvas是一个H5的标签，可以在内部通过js来绘制图形,绘制的图形都是==位图==

也仅仅是一个标签，绘图原理是：在canvas内部我们可以看做有一个透明的，高清的塑料布，在上面进行绘制

1. 给canvas设置宽高：

    canvas标签的宽高默认是300*150,是一个行内块元素

    可以在canvas标签上通过width，height来设置

    可以在js中给dom对象设置：

    ```
        mycanvas.width = 500
		mycanvas.height = 500
    ```

    切记，不要通过css来调整canvas的宽高,导致内部的canvas画布被拉伸，图形变形

2. 获取画笔工具：

    canvas绘图都是通过canvas标签的画笔来进行的

    ```
    var ctx = canvas.getContext('2d')
    ```
    注意，不要写成getContent，里面传入的参数目前也只有2d这一种情况

3. 描边和填充

    canvas绘制图形都是绘制的路径，看不见摸不着的一种东西，需要进行描边或填充之后才能看到真正的图形

    如果绘制图形的路径在绘制完成后没有闭合，继续绘制路径的时候会首尾相连

    在填充的时候如果路径依然没有闭合，会将路径的闭合区域填充

    ```
    ctx.fill()//填充
    ctx.stroke()//描边
    ```

    可以调整ctx.strokeStyle,ctx.fillStyle属性来更改填充，描边的颜色，值为颜色值（rgb,rgba,word,16进制）

4. 绘制矩形

    ctx.rect(x,y,w,h)

    canvas的坐标系起点是左上角，x轴向右正方向，y轴向下正方向

    x,y代表的是矩形起点（左上角）的位置，w,h就是宽高

    可以使用strokeRect,fillRect方法直接绘制一个填充、描边的矩形

5. 清楚矩形区域以及动画原理

    ctx.clearRect(x,y,w,h)可以清除某一个矩形区域的图形

    canvas实现动画的原理就是不断的绘制和擦除来实现

    ```
        var ctx = canvas.getContext("2d")
		let x=0,y=0;
		ctx.fillRect(x,y,50,50)	
		setInterval(function () {
			//绘制新的图形前擦掉之前的
			ctx.clearRect(0,0,canvas.width,canvas.height)
			x++;y++;
			//不断绘制新图形
			ctx.fillRect(x,y,50,50)
		},30)
    ```

6. 绘制圆弧、圆

    ctx.arc(x,y,r,startangle,endangle,isAntiClockWise)
    x,y是圆心的位置，r是半径，startangle，endangle是开始弧度和结束弧度，最后一个参数是是否是逆时针，默认是false


7. 强行闭合路径

    有的时候绘制完成一个不闭合路径之后，又去绘制其他图形了。结果两个图形的路径连接在一起出现问题

    使用ctx.beginPath() 和ctx.closePath()两个方法将某一次绘制包裹起来后，此次绘制的路径会自动闭合

8. 绘制线段

    可以使用moveTo(x,y)，lineTo(x,y)的方法来绘制线段

    moveTo绘制起点，后面可以有多个lineTo来确定其他的点，如果没有moveTo，默认第一个lineTo绘制的就是起点

    通过lineWidth调整线宽，调整lineCap(butt,round,square)来更改接头的样式

9. save和restore
    可以通过save方法和restore方法来恢复画笔设置，减少产生bug的几率

    save方法会保存此时画笔的设置，restore会恢复离的最近的save保存的设置

    ```
        ctx.save()//保存当前画笔的状态
		ctx.fillStyle = 'red'
		ctx.fillRect(0,0,100,100)
		ctx.restore()//恢复离的最近的save保存的状态
    ``
10. 曲线

    canvas里提供了两种曲线的设置方法：贝塞尔曲线，二次样条曲线

    ctx.bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y)

    cp1x,cp1y确定第一个控制点，cp2x，cp2y确定的是第二个控制点，x，y确定的是这条曲线的终点

    ctx.quadraticCurveTo(cpx,cpy,x,y)

    cpx,cpy确定控制点的坐标，x，y确定终点坐标

    线条会像各个控制点的方向弯曲来形成曲线

    ```
        ctx.moveTo(0,0)
        ctx.bezierCurveTo(50,300,300,300,300,300)
        ctx.quadraticCurveTo(50,300,300,300)

        ctx.stroke()
    ```

11. 渐变

    在canvas中，可以通过createLinearGradient、createRadialGradient创建线性、径向渐变

    let lg = ctx.createLinearGradient(x1,y1,x2,y2)四个参数确定两个点，这两点点之间就是颜色渐变的区域

    这个方法返回的是一个颜色值,这个颜色对象有addColorStop方法来添加颜色

    lg.addColorStop(offset,color);offset是颜色的偏移量（颜色最深的地方），color是这个地方的颜色

    ```
        let lg = ctx.createLinearGradient(50,0,250,0)

		lg.addColorStop(0,'red')
		lg.addColorStop(0.5,'yellow')
		lg.addColorStop(1,'blue')

		ctx.fillStyle = lg
		ctx.fillRect(0,0,300,300)
    ```

    注意，渐变的颜色、区域都是提前定义好的，我们在设置的时候很可能只是显示出某一个区域的颜色渐变情况。

    ctx2.createRadialGradient(x0,y0,r0,x1,y1,r1)可以创建径向渐变

    六个参数确定两个圆，第一个是发散开始圆，第二个是发散结束圆

    ```
        var rg = ctx2.createRadialGradient(50,150,50,200,150,150)

		rg.addColorStop(0,'red')
		rg.addColorStop(0.5,'yellow')
		rg.addColorStop(1,'blue')

		ctx2.fillStyle = rg
		ctx2.fillRect(0,0,300,300)
    ```

12. 图形整合方式

    可以设置ctx.globalCompositeOperation值来控制新图形和原有图形叠加的情况
    ```
    var arr=[
        {type:'source-over',des:'在原有图形上绘制新图形'},
        {type:'destination-over',des:'在原有图形下绘制新图形'},
        {type:'source-in',des:'显示原有图形和新图形的交集，新图形在上，所以颜色为新图形的颜色'},
        {type:'destination-in',des:'显示原有图形和新图形的交集，原有图形在上，所以颜色为原有图形的颜色'},
        {type:'source-out',des:'只显示新图形非交集部分'},
        {type:'destination-out',des:'只显示原有图形非交集部分'},
        {type:'source-atop',des:'显示原有图形和交集部分，新图形在上，所以交集部分的颜色为新图形的颜色'},
        {type:'destination-atop',des:'显示新图形和交集部分，新图形在下，所以交集部分的颜色为原有图形的颜色'},
        {type:'lighter',des:'原有图形和新图形都显示，交集部分做颜色叠加'},
        {type:'xor',des:'重叠部分不显示'},
        {type:'copy',des:'只显示新图形'}
        
    ]
    ```

13. 绘制处理图片

    canvas可以绘制出图片，并且对图片进行一系列的处理

    画笔工具的drawImage方法可以对图片进行绘制

    第一种方法：ctx.drawImage(img,x,y,w,h)；img为要绘制的图片对象，后面四个参数确定这张图片绘制在canvas画布上的哪个地方

    第二种方法:ctx.drawImage(img,x1,y1,w1,h1,x,y,w,h)x,y,w,h确定绘制在画布上的哪个地方，前四个参数确定要绘制的是图片上的哪一个区域

    ```
        var img = new Image()
        img.src='./qqb.jpg'
        img.onload = function () {
            // ctx.drawImage(img,0,0,300,150)
            ctx.drawImage(img,214,213,214,213,0,0,300,150)
        }
    ```

    toDateUrl可以将canvas图形转换成base64编码，getImageData获取到图形像素点的信息

14. 图形阴影

    在canvas中可以像css3的box-shadow属性一样为图形绘制阴影

    ```
    ctx.shadowOffsetX = "50"; //  阴影x轴偏移量
    ctx.shadowOffsetY = "5";//		y轴的偏移量
    ctx.shadowBlur = "5";	//		模糊半径
    ctx.shadowColor = "red";//		阴影颜色
    ```
15. 绘制文字

    ctx.font="normal 30px 微软雅黑";//设置字体的样式，顺序不能改style,size,family
    ctx.textBaseline = "middle";// top  bottom  middle 设置垂直对齐方式
    ctx.textAlign = "center";//设置水平对齐方式   left right center

    // ctx.fillText("千锋H5",0,0);//（实心字）
    ctx.strokeText("千锋H5",150,150)//（字的轮廓）;

16. 变形

    canvas可以通过对ctx设置translate，scale，rotate来对画布进行变形，注意的是，先旋转后唯一和先位移后旋转是不一样的

    ```
        ctx.translate(x,y)//x，y轴偏移量
        ctx.rotate(angel)//传入旋转的弧度
        ctx.scale(x,y)//x，y轴缩放的倍数
    ```


## 数据可视化

将数据以图表等格式，输出到页面中！

一般情况会使用基于canvas的数据可视化插件：

echarts、highcharts、d3....

当数据变化之后，进行重新显示！

echarts：setOptions(new_option)

websocket

到后端数据变化之后，前端就得知了，将数据改吧改吧，重新setOptions