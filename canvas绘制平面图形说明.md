## 直线的绘制
### 普通直线
cxt.moveTo( x1, y1 )： 将画笔移动到x1, y1这个点
cxt.lineTo( x2, y2 )：将画笔从起点开始画直线，一直画到终点坐标( x2, y2 )
cxt.stroke();用画笔连线，moveTo,lineTo并不会产生实际的线条
x1,y1,x2,y2是点的坐标，canvas的坐标原点在canvas的左上角.
### 带箭头的直线
在Canvas的CanvasRenderingContext2D对象中是没有提供绘制箭头的方法，言外之意，在Canvas中要绘制箭头是需要自己封装函数来处理。
这里我们传了九个参数：
* ctx：Canvas绘图环境
* fromX, fromY：起点坐标（也可以换成p1，只不过它是一个数组）
* toX, toY：终点坐标 (也可以换成p2，只不过它是一个数组)
* theta：三角斜边一直线夹角
* headlen：三角斜边长度
* width：箭头线宽度
* color：箭头颜色
```
function drawArrow(ctx, fromX, fromY, toX, toY,theta,headlen,width,color) {/* 封装绘制箭头函数 */
				
				theta = typeof(theta) != 'undefined' ? theta : 30;
				headlen = typeof(theta) != 'undefined' ? headlen : 10;
				width = typeof(width) != 'undefined' ? width : 1;
				color = typeof(color) != 'color' ? color : '#000';
				
				// 计算各角度和对应的P2,P3坐标
				var angle = Math.atan2(fromY - toY, fromX - toX) * 180 / Math.PI,
					angle1 = (angle + theta) * Math.PI / 180,/* 箭头两边侧边线的夹角 */
					angle2 = (angle - theta) * Math.PI / 180,
					topX = headlen * Math.cos(angle1),
					topY = headlen * Math.sin(angle1),
					botX = headlen * Math.cos(angle2),
					botY = headlen * Math.sin(angle2);
					
				
					ctx.save();
					ctx.beginPath();
					
				var arrowX = fromX - topX,
					arrowY = fromY - topY;
				ctx.moveTo(arrowX, arrowY);
				ctx.moveTo(fromX, fromY);
				ctx.lineTo(toX, toY);
				arrowX = toX + topX;
				arrowY = toY + topY;
				ctx.moveTo(arrowX, arrowY);
				ctx.lineTo(toX, toY);
				arrowX = toX + botX;
				arrowY = toY + botY;
				ctx.lineTo(arrowX, arrowY);
				ctx.strokeStyle = color;
				ctx.lineWidth = width;
				ctx.stroke();
				ctx.restore();
			  }
```
drawArrow(ctx, myCanvas.width / 2, myCanvas.height / 2, myCanvas.width / 2 + 150, myCanvas.height / 2,30,30,4,'#f36');/* 一条向右的箭头 */
[参考](https://www.w3cplus.com/canvas/drawing-arrow.html?utm_source=tuicool&utm_medium=referral)
### 虚线
线性路径的绘制，主要利用movoTo()，lineTo()等方法，当然 Canvas 2D API 也提供了虚线的绘制方法，CanvasRenderingContext2D.setLineDash();
语法：ctx.setLineDash(segments);
[参考](https://blog.csdn.net/qq_32135281/article/details/73866238)
## 其他图形的绘制
矩形、圆、椭圆
参考菜鸟教程官网
圆：arc(x,y,r,start,stop)

矩形：context.rect(x,y,width,height)

椭圆：ellipse(x, y, radiusX, radiusY, rotation, startAngle, endAngle, anticlockwise)是现在更新的
参数的意思：(起点x.起点y,半径x,半径y,旋转的角度，起始角，结果角，顺时针还是逆时针)（目前仅谷歌浏览器支持）
[参考](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Drawing_shapes)