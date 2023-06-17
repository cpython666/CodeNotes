步骤：

1. 引入js
2. 定义dom节点

```js
<div class="daily-count-chart" style="width: 650px;height: 500px"></div>
```

3. 获取dom节点

```js
let myChart = echarts.init(document.querySelector(".chart"))
```

4. 定义配置

```js
option = {
};
```

5. 设置

```js
myChart.setOption(option);
```

## 折线图：

```js
let option = {
          xAxis: {
            type: 'category',
            data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
          },
          yAxis: {
            type: 'value'
          },
          series: [
            {
              data: [820, 932, 901, 934, 1290, 1330, 1320],
              type: 'line',
              smooth: true,
                color:'blue',
                markPoint: {
                data: [
                  { type: 'max', name: 'Max' },
                  { type: 'min', name: 'Min' }
                ]
              },
            }
          ]
        };
```

