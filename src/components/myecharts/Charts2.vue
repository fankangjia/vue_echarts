<template>
  <div id='myChart2' :style="{ width: blockWidth, height: blockHeight}"></div>
</template>

<script>

//先要导入依赖的实例
import echarts from 'echarts'
 export default {
    name: 'Charts2',
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
    methods: {
      seriesDate() {
         let data= [{value:235, name:'漏气'},{value:275, name:'点火'},{value:310, name:'换表'},{value:335, name:'欠费'},{value:400, name:'上门维修'}];
         return data;
      }
    },
    data () {
      return {
        echarts1_option:{
          tooltip : {
              trigger: 'item',
              // formatter: "{a} <br/>{b} : {c} ({d}%)"
          },
          series : [{
                name: '访问来源',
                type: 'pie',
                radius: '80%',
                data:[],
                roseType: 'angle',
                label: {
                    normal: {
                        textStyle: {
                            color: 'rgba(38, 198, 248, 1)',
                            fontSize: '15',
                            fontWeight: 'bold'
                        }
                    }
                },
                labelLine: {
                    normal: {
                        lineStyle: {
                            color: 'rgba(38, 198, 248, 0.7)'
                        }
                    }
                },
                itemStyle: {
                    normal: {
                        shadowBlur: 200,
                        shadowColor: 'rgba(38, 198, 248, 0.5)'
                    }
                }
            }]
        },
        chartcontainer:''
      }
    },
    //挂载前初始化echarts实例
    mounted: function () {
      // 基于准备好的dom，初始化echarts实例
      this.chartcontainer = echarts.init(document.getElementById('myChart2'),'light')
      // 绘制图表，this.echarts1_option是数据
      this.chartcontainer.setOption(this.echarts1_option)
      //添加异步的数据
      let chartData=this.seriesDate();
      this.chartcontainer.setOption({
        series : [{
              data:chartData
          }]
      })
    },
 }
</script>

<style scoped>

</style>
