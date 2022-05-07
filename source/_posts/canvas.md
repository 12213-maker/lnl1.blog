
---
title: Canvas
---
  ## canvas可以绘制路径 , 矩形 , 圆形, 字符  , 渐变, 以及 添加图像

首先要创建一个canvas容器
```javascript
<canvas id="myCanvas" width="300" height="300" ></canvas>
```
获取容器 , 并创建context对象

```javascript
var c=document.getElementById("myCanvas");
//创建context对象
var ctx=c.getContext("2d");
```

- 路径(直线)
		- 方法:  ``moveTo(x,y)``   	  ``lineTo(x,y)``    x,y确定在画布里的起始位置和结束位置
  		- 使用  ``stroke()`` 方法来绘制

```javascript

var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
ctx.moveTo(0,0);
ctx.lineTo(200,100);
ctx.stroke();
```

- 矩形
		- 方法 : ``fillStyle="color"``   ``fillRect(x,y,width,height)``  x,y 矩形的起点 , width , height 矩形的宽高 
```javascript
ctx.fillStyle="blue";
ctx.fillRect(150,150,150,150);
```

- 圆形
		- 方法: ``arc(150,150,75,0,2*Math.PI)`` //x,y,r,srart,stop  `` stroke()``
```javascript
ctx.arc(150,150,75,0,2*Math.PI) //x,y,r,srart,stop
ctx.stroke()
```

- 文本字符
		- 方法: ``ctx.font = "30px Arial" ``字体效果      ``strokeText("一十四洲",150,150)``内容 , x,y起点
```javascript
ctx.font = "30px Arial"
ctx.strokeText("一十四洲",150,150)//内容 , x,y起点
```

- 渐变
		- 方法:  
					- createLinearGradient() // 线性渐变
					- createRadialGradient() //径向渐变
					- addColorStop() 
```javascript
// var grd1 = ctx.createLinearGradient(0,0,200,0); //线性渐变
var grd=ctx.createRadialGradient(75,50,5,90,60,100);//径向渐变
grd.addColorStop(0,'red')
grd.addColorStop(1,'white')
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/5e74cede1bf74686b426a5570ed67466.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiN5Li66Zyc5YGc,size_18,color_FFFFFF,t_70,g_se,x_16#pic_center)