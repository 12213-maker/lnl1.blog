---

title: 初识Echarts

---

### 安装echarts

```javascript
npm install echarts --save
```
### 在mian.js中引入(全局引入)
```javascript
import * as echarts from "echarts"
Vue.prototype.$echarts = echarts
```
### 开始使用echarts
1. 定义有``宽度和高度``的父容器
2. 初始化``echarts实例``
3. 指定配置项和数据
4. 将配置项设置给echarts实例对象 绘制图表

```javascript
1. 定义有宽度和高度的父容器
<div id="main" class="main_container"></div>

2. 初始化echarts实例
var mycharts = this.$echarts.init(document.getElementById("main"))

3. 指定配置项和数据
const option={}

4. 将配置项设置给echarts实例对象 绘制图表
mycharts.setOption(option)
```
### 基础配置(摘抄自黑马教程)
- ``series``
	- 系列列表 , 每个系列通过``type``决定自己的图标类型 , 可以指定图标数据 , 可以多个图标重叠
- ``xAxis``
	- boundaryGap: 坐标轴两边的留白策略true , 这时候刻度只是作为分割线 , 标签和数据点都会在两个刻度之间的带(band)中间
- ``yAxis`` : 直角 grid 中的 y 轴
-`` grid`` : 直角坐标系内绘图网络
- ``title`` : 标题组件
-`` tooltip`` : 提示框组件
-`` legend`` : 图例组件
- ``color`` : 调色盘颜色列表
	数据堆叠 , 同个类目轴上系列配置相同的``stack``值后 , 后一个系列的值会在前一个系列的值上相加
```javascript
/* 设置配置项 */
    const option = {
      /* color设置我们的线条的颜色 , 注意后面是一个数组 */
      color:['red','green'],
      /* 标题 */
      title: {
        text: "Temperature Change",
      },
      /* 图表的提示框组件 */
      tooltip: {
        /* 触发方式 , 折线图是axis , 柱状图是item */
        trigger: "axis",
      },
      legend: {
        /* series里面有name的话就不需要legend */
      },
      /* 网格配置 grid可以控制线性图 , 柱状图 , 图标大小 */
      // grid:{
      //   /* 图标距离左边的距离 */
      //   left:'20%',
      //   right:'20%',
      //   top:'20%',
      //   /* 是否显示刻度标签 */
      //   containLabel:true
      // },
      /* 工具箱组件 , 可以另存为图片等功能 */
      toolbox: {
        show: true,
        feature: {
          dataZoom: {
            yAxisIndex: "none",
          },
          dataView: { readOnly: false },
          magicType: { type: ["line", "bar"] },
          restore: {},
          saveAsImage: {},
        },
      },
      /* 设置x轴的相关配置 */
      xAxis: {
        /* 类目轴 , 要搭配data使用 , 自定义 x轴 */
        type: "category",
        /* 是否让我们的线条和坐标轴有缝隙 */
        boundaryGap: false,
        /* 自定义坐标轴 */
        data: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
        /* 设置x轴标签文字样式 */
        axisLabel:{
          color:'red',
        },
        /* x轴样式不显示 */
        axisLine:{
          show:false
        }
      },
      yAxis: {
        type: "value",
        axisLabel: {
          formatter: "{value} °C",
          /* 把刻度标签里面的文字颜色设置为白色 */
          color:'white',
        },
        /* y轴的分割线 */
        // splitLine:{
        //   lineStyle:{
        //     color:'blue'
        //   }
        // }

        /* 不显示刻度 */
        axisTick:{
          show:false
        },
        /* 不显示y轴的线 */
        axisLine:{
          show:false
        },

      },
      /* 系列图表配置 , 它决定着显示哪种类型的图表 */
      series: [
        {
          name: "Highest",
          type: "line",
          data: this.datavalue,
          markPoint: {
            data: [
              { type: "max", name: "Max" },
              { type: "min", name: "Min" },
            ],
          },
          markLine: {
            data: [{ type: "average", name: "Avg" }],
          },
        },
        {
          name: "Lowest",
          type: "line",
          data: [1, -2, 2, 5, 3, 2, 0],
          markPoint: {
            data: [{ name: "周最低", value: -2, xAxis: 1, yAxis: -1.5 }],
          },
          markLine: {
            data: [
              { type: "average", name: "Avg" },
              [
                {
                  symbol: "none",
                  x: "90%",
                  yAxis: "max",
                },
                {
                  symbol: "circle",
                  label: {
                    position: "start",
                    formatter: "Max",
                  },
                  type: "max",
                  name: "最高点",
                },
              ],
            ],
          },
        },
      ],
    };
  
```
### 让echarts实现响应式
在mounted中利用``window.onresize``来监听页面是否发生变化 , 当页面发生变化的时候 , 就重绘图表 , 但是注意在 window.onresize 的``funciton中this指向不再是vm`` , 所以就要在之前``let _this = this`` , 并且为了在 mounted 中获得 echarts 实例对象 mycharts  , 就要在 data 中定义 mycharts  , 在实例化的时候直接``this.mycharts = this.$echarts.init()``
```javascript
 data() {
    return {
      datavalue:[10, 11, 13, 11, 12, 12, 9],
      mycharts:{}
    };
  },
  mounted() {
    this.Echarts()
    /* 图表响应式 */
    let _this = this
    window.onresize = function(){
      _this.mycharts.resize()
    }
  },
```