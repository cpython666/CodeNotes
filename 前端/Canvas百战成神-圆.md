# Canvas百战成神-圆

## 初始化容器

```html
<canvas id="canvas"></canvas>

canvas{
    border: 1px solid black;
}
```

## 让页面占满屏幕

```css
*{
    margin: 0;
    padding: 0;
}
html,body{
    width: 100%;
    height: 100%;
    overflow: hidden;
}
::-webkit-scrollbar{
    display: none;
}
```



## 初始化画笔

```js
let canvas=document.querySelector('canvas')
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
let c=canvas.getContext('2d');
```

## 以(x,y)为圆心，r为半径画一个圆

![image-20230317224503384](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/image-20230317224503384.png)

```js
var x=20;
var y=20;
var r=20;
c.beginPath()
c.arc(x,y,r,Math.PI*2,false);
c.strokeStyle='blue';
c.stroke();
```

## 创建一个动画帧

![canvas-1](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-1.gif)

```js
function animate(){
    requestAnimationFrame(animate);
    console.log(1)
}
animate()
```

## 让圆以速度vx直线运动

![canvas-2](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-2.gif)

```js
var x=20;
var y=20;
var r=10;
var vx=1;
var width=canvas.width
var height=canvas.height

function animate(){
    requestAnimationFrame(animate);

    c.clearRect(0,0,width,height);

    c.beginPath()
    c.arc(x,y,r,Math.PI*2,false);
    c.strokeStyle='blue';
    c.stroke();

    x+=vx
}
animate()
```

## 当圆碰到边界时反弹

![canvas-2](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-2-16790696645323.gif)

```js
var x=30;
var y=30;
var r=20;
var vx=4;
var width=canvas.width
var height=canvas.height

function animate(){
    requestAnimationFrame(animate);

    c.clearRect(0,0,width,height);

    c.beginPath()
    c.arc(x,y,r,Math.PI*2,false);
    c.strokeStyle='blue';
    c.stroke();
//到达边界时速度变为相反数
    if(x-r<0 || x+r>width){
        vx=-1*vx
    }

    x+=vx
}
animate()
```

## 给圆加上一个y轴的速度

![canvas-3](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-3.gif)

```js
//添加一个y轴的速度
var x=30;
var y=30;
var r=20;
var vx=4;
var vy=3;
var width=canvas.width
var height=canvas.height

function animate(){
    requestAnimationFrame(animate);

    c.clearRect(0,0,width,height);

    c.beginPath()
    c.arc(x,y,r,Math.PI*2,false);
    c.strokeStyle='blue';
    c.stroke();

    if(x-r<0 || x+r>width){
        vx=-1*vx
    }
    if(y-r<0 || y+r>height){
        vy=-1*vy
    }

    x+=vx
    y+=vy
}
animate()
```

## 令圆的大小,生成点,速度随机

![canvas-5](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-5.gif)

```js
//加上随机数
var r=(Math.random()+0.5)*10+10;
var width=canvas.width
var height=canvas.height
var x=Math.random()*(width-2*r)+r;
var y=Math.random()*(height-2*r)+r
var vx=(Math.random()-0.5)*8;
var vy=(Math.random()-0.5)*8;

function animate(){
    requestAnimationFrame(animate);

    c.clearRect(0,0,width,height);

    c.beginPath()
    c.arc(x,y,r,Math.PI*2,false);
    c.strokeStyle='blue';
    c.stroke();

    if(x-r<0 || x+r>width){
        vx=-1*vx
    }
    if(y-r<0 || y+r>height){
        vy=-1*vy
    }

    x+=vx
    y+=vy
}
animate()
```

## 函数化，批量化生成圆

![canvas-6](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-6.gif)

```js
function Circle(x,y,vx,vy,r){
    this.x=x;
    this.y=y;
    this.vx=vx;
    this.vy=vy;
    this.r=r;

    this.draw=function(){
        c.beginPath()
        c.arc(this.x,this.y,this.r,Math.PI*2,false);
        c.strokeStyle='blue';
        c.stroke();
    }

    this.update=function(){
        if(this.x-this.r<0 || this.x+this.r>width){
            this.vx=-1*this.vx
        }
        if(this.y-this.r<0 || this.y+this.r>height){
            this.vy=-1*this.vy
        }
        this.x+=this.vx
        this.y+=this.vy
        this.draw()
    }
}

var width=canvas.width
var height=canvas.height
var circleArray=[]
for (var i=0;i<10;i++){
    var r=(Math.random()+0.5)*10+10;
    var x=Math.random()*(width-2*r)+r;
    var y=Math.random()*(height-2*r)+r
    var vx=(Math.random()-0.5)*8;
    var vy=(Math.random()-0.5)*8;

    circleArray.push(new Circle(x,y,vx,vy,r))
}

function animate(){
    requestAnimationFrame(animate);

    c.clearRect(0,0,width,height);

    for (var i=0;i<circleArray.length;i++){
        circleArray[i].update()
    }
}
animate()
```

## 实心圆

![canvas-7](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-7.gif)

```js
//在Circle的draw()方法后写上c.fill()方法
this.draw=function(){
    c.beginPath()
    c.arc(this.x,this.y,this.r,Math.PI*2,false);
    c.strokeStyle='blue';
    c.stroke();
    c.fillStyle=colorArray[Math.floor(Math.random()*colorArray.length)]
    c.fill()
}
```

## 靠近鼠标的圆变大,远离再变小

![canvas-8](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-8.gif)

```js
var maxRadius=40
var minRadius=4

var mouse={
    x:undefined,
    y:undefined
}

window.addEventListener("mousemove", function(event){
    mouse.x = event.x
    mouse.y = event.y
})
```

```js
//在update方法里加上判断
if(mouse.x-this.x<50 && mouse.x-this.x>-50 && mouse.y-this.y<50 && mouse.y-this.y>-50){
    if(this.r<maxRadius){
        this.r+=1
    }
}else if(this.r>minRadius){
    this.r-=1
}
```

## 给圆加上随机颜色

![canvas-9](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-9.gif)

```js
var colorArray=[
    'red',
    'blue',
    'pink',
    'orange',
    'purple',
    'green',
    'yellow',
]
```

```js
//在draw方法里加上颜色
c.fillStyle=colorArray[Math.floor(Math.random()*colorArray.length)]
```

## 将随机的颜色变为圆固定的属性

![canvas-11](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-11.gif)

```js
this.color=colorArray[Math.floor(Math.random()*colorArray.length)]
```

```js
//draw方法里填充自己的颜色
c.fillStyle=this.color
```

## 为每个圆设置最大最小半径

![canvas-12](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-12.gif)

```js
//完整代码如下
var canvas = document.getElementById('canvas');
let c=canvas.getContext('2d');
var width=canvas.width = window.innerWidth;
var height=canvas.height = window.innerHeight;

var mouse={
    x:undefined,
    y:undefined
}
var colorArray=[
    'red',
    'blue',
    'pink',
    'orange',
    'purple',
    'green',
    'yellow',
]
var circleArray=[]

window.addEventListener("mousemove", function(event){
    mouse.x = event.x
    mouse.y = event.y
})

function Circle(x,y,vx,vy,r,maxRadius,minRadius){
    this.x=x;
    this.y=y;
    this.vx=vx;
    this.vy=vy;
    this.r=r;

    this.maxRadius=maxRadius
    this.minRadius=minRadius

    this.color=colorArray[Math.floor(Math.random()*colorArray.length)]

    this.draw=function(){
        c.beginPath()
        c.arc(this.x,this.y,this.r,Math.PI*2,false);
        c.strokeStyle='blue';
        c.stroke();
        c.fillStyle=this.color
        c.fill()
    }

    this.update=function(){
        if(this.x-this.r<0 || this.x+this.r>width){
            this.vx=-1*this.vx
        }
        if(this.y-this.r<0 || this.y+this.r>height){
            this.vy=-1*this.vy
        }
        this.x+=this.vx
        this.y+=this.vy

        if(mouse.x-this.x<50 && mouse.x-this.x>-50 && mouse.y-this.y<50 && mouse.y-this.y>-50){
            if(this.r<this.maxRadius){
                this.r+=1
            }
        }else if(this.r>this.minRadius){
            this.r-=1
        }

        this.draw()
    }
}

for (var i=0;i<200;i++){
    var r=(Math.random()+0.5)*10+30;
    var maxRadius=(Math.random()+0.5)*10+20;
    var minRadius=(Math.random()+0.5)*4+1;
    var x=Math.random()*(width-2*r)+r;
    var y=Math.random()*(height-2*r)+r
    var vx=(Math.random()-0.5)*2;
    var vy=(Math.random()-0.5)*2;

    circleArray.push(new Circle(x,y,vx,vy,r,maxRadius,minRadius))
}
function animate(){
    requestAnimationFrame(animate);

    c.clearRect(0,0,width,height);

    for (var i=0;i<circleArray.length;i++){
        circleArray[i].update()
    }
}
animate()
```

## 增多数量

![canvas-13](Canvas%E7%99%BE%E6%88%98%E6%88%90%E7%A5%9E-%E5%9C%86.assets/canvas-13.gif)

```js
for (var i=0;i<800;i++){
}
```

## 窗口大小改变时刷新

```js
var canvas = document.getElementById('canvas');
let c=canvas.getContext('2d');
var width=canvas.width = window.innerWidth;
var height=canvas.height = window.innerHeight;

var mouse={
    x:undefined,
    y:undefined
}
var colorArray=[
    'red',
    'blue',
    'pink',
    'orange',
    'purple',
    'green',
    'yellow',
]
var circleArray=[]

window.addEventListener("mousemove", function(event){
    mouse.x = event.x
    mouse.y = event.y
})
window.addEventListener("resize", function(event){
    width=canvas.width = window.innerWidth;
    height=canvas.height = window.innerHeight;

    init();
})
function Circle(x,y,vx,vy,r,maxRadius,minRadius){
    this.x=x;
    this.y=y;
    this.vx=vx;
    this.vy=vy;
    this.r=r;

    this.maxRadius=maxRadius
    this.minRadius=minRadius

    this.color=colorArray[Math.floor(Math.random()*colorArray.length)]

    this.draw=function(){
        c.beginPath()
        c.arc(this.x,this.y,this.r,Math.PI*2,false);
        c.strokeStyle='blue';
        c.stroke();
        c.fillStyle=this.color
        c.fill()
    }

    this.update=function(){
        if(this.x-this.r<0 || this.x+this.r>width){
            this.vx=-1*this.vx
        }
        if(this.y-this.r<0 || this.y+this.r>height){
            this.vy=-1*this.vy
        }
        this.x+=this.vx
        this.y+=this.vy

        if(mouse.x-this.x<50 && mouse.x-this.x>-50 && mouse.y-this.y<50 && mouse.y-this.y>-50){
            if(this.r<this.maxRadius){
                this.r+=1
            }
        }else if(this.r>this.minRadius){
            this.r-=1
        }

        this.draw()
    }
}


function init(){
    circleArray=[]
    for (var i=0;i<800;i++){
        var r=(Math.random()+0.5)*10+30;
        var maxRadius=(Math.random()+0.5)*10+20;
        var minRadius=(Math.random()+0.5)*4+1;
        var x=Math.random()*(width-2*r)+r;
        var y=Math.random()*(height-2*r)+r
        var vx=(Math.random()-0.5)*2;
        var vy=(Math.random()-0.5)*2;

        circleArray.push(new Circle(x,y,vx,vy,r,maxRadius,minRadius))
    }
}
function animate(){
    requestAnimationFrame(animate);

    c.clearRect(0,0,width,height);

    for (var i=0;i<circleArray.length;i++){
        circleArray[i].update()
    }
}
init()
animate()
```

