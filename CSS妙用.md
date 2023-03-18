## 动态纹理背景

效果：

![css1](CSS%E5%A6%99%E7%94%A8.assets/css1.gif)

代码：

```css
.box{
    width:200px;
    height:200px;
    background: linear-gradient(45deg, blue 10%, transparent 10%,transparent 50%, blue 50%, blue 60%, transparent 60%, transparent 100%);
    background-size: 40px 40px;
    animation: animate 0.5s linear infinite;
    -webkit-box-reflect: below 2px linear-gradient(transparent, #0005);
}
@keyframes animate
{
    0%
    {background-position: 0;}
    100%
    {background-position: 40px;}
}
```

## 动态文字变化

效果：

![css2](CSS%E5%A6%99%E7%94%A8.assets/css2.gif)

代码：

```css
.sport-text{
    font: bold 100px Calibri;
    background-size: 50px 50px;
    background-image: linear-gradient(
        135deg,
        white 25%,red 25%,red 50%,
        white 50%,white 75%,red 75%);
    animation:animate 2s linear 0s infinite;
    -webkit-background-clip: text;
    -webkit-text-stroke: 1px white;
    color: transparent;
}
@keyframes animate {
    0%
    {background-position: 0;}
    100%
    {background-position: 50px;}
}
```

## 文字颜色渐变

效果：

![image-20230316160703632](CSS%E5%A6%99%E7%94%A8.assets/image-20230316160703632.png)

代码：

```css
.text{
    font:bold 20px Calibri;
    background-image: -webkit-linear-gradient(
        0deg,
        rgb(239, 18, 136) ,
        rgb(56, 15, 239) 10%,
        rgb(233, 42, 9) 50%,
        rgb(9, 162, 245) 95%
    );
    -webkit-background-clip: text;
    color: transparent;
}
```

## 文字氙灯效果

效果：

![css3](CSS%E5%A6%99%E7%94%A8.assets/css3.gif)

代码：

```css
.a {
    color: #88e;
    text-shadow: 0 0 0.3em rgba(200, 200, 255, 0.3), 0.04em 0.04em 0 #112,
        0.045em 0.045em 0 #88e, 0.09em 0.09em 0 #112, 0.095em 0.095em 0 #66c,
        0.14em 0.14em 0 #112, 0.145em 0.145em 0 #44a;
    animation: pulsea 300ms ease infinite alternate;
}
@keyframes pulsea {
    0% {
        text-shadow: 0 0 0.3em rgba(200, 200, 255, 0.3), 0.04em 0.04em 0 #112,
            0.045em 0.045em 0 #88e, 0.09em 0.09em 0 #112, 0.095em 0.095em 0 #66c,
            0.14em 0.14em 0 #112, 0.145em 0.145em 0 #aaf;
    }
    50% {
        text-shadow: 0 0 0.3em rgba(200, 200, 255, 0.3), 0.04em 0.04em 0 #112,
            0.045em 0.045em 0 #88e, 0.09em 0.09em 0 #112, 0.095em 0.095em 0 #aaf,
            0.14em 0.14em 0 #112, 0.145em 0.145em 0 #44a;
    }
    75% {
        text-shadow: 0 0 0.3em rgba(200, 200, 255, 0.3), 0.04em 0.04em 0 #112,
            0.045em 0.045em 0 #aaf, 0.09em 0.09em 0 #112, 0.095em 0.095em 0 #66c,
            0.14em 0.14em 0 #112, 0.145em 0.145em 0 #44a;
    }
    100% {
        text-shadow: 0 0 0.3em rgba(200, 200, 255, 0.4), 0.04em 0.04em 0 #112,
            0.045em 0.045em 0 #88e, 0.09em 0.09em 0 #112, 0.095em 0.095em 0 #66c,
            0.14em 0.14em 0 #112, 0.145em 0.145em 0 #44a;
    }
}
```

