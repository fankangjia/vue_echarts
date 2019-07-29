<template>
  <div id='myChart2' :style="{ width: blockWidth, height: blockHeight}"></div>
</template>

<script>

//先要导入依赖的实例
import echarts from 'echarts'
import axios from 'axios'
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
    data () {
      return {
        echarts1_option:{
          tooltip: {
              trigger: 'item',
              formatter: "{a} <br/>{b}: {c} ({d}%)"
          },
          legend: {
              orient: 'vertical',
              x: 'left',
              data:[],
              textStyle: {
                  color: 'rgba(38, 198, 248, 1)'
              }
          },
          series: [
              {
                  name:'详情',
                  type:'pie',
                  radius: ['55%', '85%'],
                   center: ['50%', '47%'],
                  avoidLabelOverlap: false,
                  label: {
                      normal: {
                          show: false,
                          position: 'center'
                      },
                      emphasis: {
                          show: true,
                          textStyle: {
                              fontSize: '30',
                              fontWeight: 'bold'
                          }
                      }
                  },
                  labelLine: {
                      normal: {
                          show: false
                      }
                  },
                  data:[ ]
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
          legend: {
              data:newValue.map((x)=>x['name'])
          },
          series : [{
                data:newValue
            }]
        })
      }
    },
    methods: {
      seriesDate() {
        var that = this
         // axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=xx&m=ns_klny')
         axios.get('../../../static/data/data.json')
          .then(function (response) {
            that.chartData=response.data;
          })
          .catch(function (error) {
            that.fetc
          });
      }
    },
    //挂载前初始化echarts实例
    mounted: function () {
      var self = this
      // 基于准备好的dom，初始化echarts实例
      this.chartcontainer = echarts.init(document.getElementById('myChart2'),'light')
      // 绘制图表，this.echarts1_option是数据
      this.chartcontainer.setOption(this.echarts1_option)
      //调用异步方法获取数据
      this.seriesDate()
    },
 }
</script>

<style scoped>

</style>
