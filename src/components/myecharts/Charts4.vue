<template>
  <div id='myChart4' :style="{ width: blockWidth, height: blockHeight}"></div>
</template>

<script>

//先要导入依赖的实例
import echarts from 'echarts'
import axios from 'axios'
 export default {
    name: 'Charts4',
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
        tooltip: {
            trigger: 'item',
            formatter: "{a} <br/>{b}: {c} ({d}%)"
        },
        series : [
              {
                  name: '详情',
                  type: 'pie',
                  radius: '70%',
                  center: ['50%', '46%'],
                  data:[],
                  roseType: 'angle',
              }
          ]
        },
        chartcontainer:'',
        chartData:[]
      }
    },
    watch: {
      chartData(newValue, oldValue) {
        console.log(newValue)
        this.chartcontainer.setOption({
          series : [
                {
                  data:newValue
                }
            ]
        })
      }
    },
    methods: {
      seriesDate() {
        var that = this
         axios.get('../../../static/data/cjr.json')
         // axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=cc&m=ns_klny')
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
      this.chartcontainer = echarts.init(document.getElementById('myChart4'),'light')
      // 绘制图表，this.echarts1_option是数据
      this.chartcontainer.setOption(this.echarts1_option)
      //调用异步方法获取数据
      this.seriesDate()
    },
 }
</script>

<style scoped>
</style>
