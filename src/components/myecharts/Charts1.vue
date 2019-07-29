<template>
  <div id='myChart1' :style="{ width: blockWidth, height: blockHeight}"></div>
</template>

<script>

//先要导入依赖的实例
import echarts from 'echarts'
import axios from 'axios'
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
           tooltip : {
              trigger: 'axis',
              axisPointer : {            // 坐标轴指示器，坐标轴触发有效
                type : 'shadow'        // 默认为直线，可选为：'line' | 'shadow'
              }
            },
            graid:{
              x:'20%',
              x2:'10%',
            },
            textStyle: {
                color: 'rgba(38, 198, 248, 1)'
            },
            legend: {
              data: ['上午', '下午'],
              textStyle: {
                  color: 'rgba(38, 198, 248, 1)'
              }
            },
            xAxis:  {
              type: 'value',
              position: 'bottom',
            },
            yAxis: {
              inverse:true,
              type: 'category',
              data: ['滨湖一','滨湖二','包河区'],
              nameLocation: 'start',
              nameGap: 30,
              axisLabel:{
                rotate:-90
              }
            },
            dataZoom: [
              { // 第一个 dataZoom 组件
                type:'inside',
                yAxisIndex: [0],// 表示这个 dataZoom 组件控制 第一个 和 第三个 yAxis
                filterMode: 'filter',
                startValue: 0,
                endValue: 4,
              },
            ],
            series: [
              {
                name: '上午',
                type: 'bar',
                "stack": '1',
                label: {
                  normal: {
                    show: true,
                    position: 'insideRight'
                  }
                },
                data: []
              },
              {
                name: '下午',
                type: 'bar',
                stack: '1',
                label: {
                  normal: {
                    show: true,
                    position: 'insideRight'
                  }
                },
                data: []
              }
            ]
        },
        chartcontainer:'',
        chartData:[]
      }
    },
    watch: {
      chartData(newValue, oldValue) {
        this.chartcontainer.setOption({
          series : [
                {
                  data:newValue[0]
                },
                {
                  data:newValue[1]
                }
            ]
        })
      }
    },
    methods: {
      seriesDate() {
        var that = this
         // axios.get('../../../static/data/y2.json')
         axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=aa&m=ns_klny')
          .then(function (response) {
            that.chartData=response.data;
          })
          .catch(function (error) {
            console.log(error)
          });
      }
    },
    //挂载前初始化echarts实例
    mounted: function () {
      // 基于准备好的dom，初始化echarts实例
      this.chartcontainer = echarts.init(document.getElementById('myChart1'))
      // 绘制图表，this.echarts1_option是数据
      this.chartcontainer.setOption(this.echarts1_option)
      //调用异步方法获取数据
      this.seriesDate()
    },
 }
</script>

<style scoped>
</style>
