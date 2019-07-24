<template>
  <div id='myChart' :style="{ width: blockWidth, height: blockHeight}"></div>
</template>

<script>

//先要导入依赖的实例
import echarts from 'echarts'
 export default {
    name: 'Charts1',
    props: {
      blockWidth: {
        type: String,
        default: '200px'
      },
      blockHeight: {
        type: String,
        default: '200px'
      },
    },
    data () {
      return {
        echarts1_option:{
          //这里写数据表的定义
            tooltip: {},
            grid:{
              x:20,
              y:20,
              x2:20,
              y2:50
            },
            xAxis: {
                data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
            },
            yAxis: {},
            series: [{
                name: '销量',
                type: 'bar',
                data: [5, 20, 36, 10, 10, 20]
            }]
        }
      }
    },
    //挂载前初始化echarts实例
    mounted: function () {
      // 基于准备好的dom，初始化echarts实例
      let myChart = echarts.init(document.getElementById('myChart'))
      // 绘制图表，this.echarts1_option是数据
      myChart.setOption(this.echarts1_option)
    },
 }
</script>

<style>
</style>
