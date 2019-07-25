<template>
  <div id='myChart1' :style="{ width: blockWidth, height: blockHeight}"></div>
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
          title: {
              text: '折线图堆叠'
          },
          textStyle: {
              color: 'rgba(38, 198, 248, 1)'
          },
          tooltip: {
              trigger: 'axis'
          },
          legend: {
              data:['邮件营销','联盟广告','视频广告','直接访问','搜索引擎']
          },
          grid: {
              left: '3%',
              right: '4%',
              bottom: '3%',
              containLabel: true
          },
          toolbox: {
              feature: {
                  saveAsImage: {}
              }
          },
          xAxis: {
              type: 'category',
              boundaryGap: false,
              data: ['周一','周二','周三','周四','周五','周六','周日']
          },
          yAxis: {
              type: 'value'
          },
          series: [
              {
                  name:'邮件营销',
                  type:'line',
                  stack: '总量',
                  data:[120, 132, 101, 134, 90, 230, 210]
              },
              {
                  name:'联盟广告',
                  type:'line',
                  stack: '总量',
                  data:[220, 182, 191, 234, 290, 330, 310]
              },
              {
                  name:'视频广告',
                  type:'line',
                  stack: '总量',
                  data:[150, 232, 201, 154, 190, 330, 410]
              },
              {
                  name:'直接访问',
                  type:'line',
                  stack: '总量',
                  data:[320, 332, 301, 334, 390, 330, 320]
              },
              {
                  name:'搜索引擎',
                  type:'line',
                  stack: '总量',
                  data:[820, 932, 901, 934, 1290, 1330, 1320]
              }
          ]
        },
        chartcontainer:''
      }
    },
    //挂载前初始化echarts实例
    mounted: function () {
      // 基于准备好的dom，初始化echarts实例
      this.chartcontainer = echarts.init(document.getElementById('myChart1'),'light')
      // 绘制图表，this.echarts1_option是数据
      this.chartcontainer.setOption(this.echarts1_option)
    },
 }
</script>

<style scoped>
</style>
