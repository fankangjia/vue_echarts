<template>
  <div id='chartH5' style="margin-left: 5px;" :style="{ width: blockWidth, height: blockHeight}"></div>
</template>

<script>

//先要导入依赖的实例
import echarts from 'echarts'
import axios from 'axios'
 export default {
    name: 'chartH5',
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
          color: ['#3398DB'],
          tooltip: {
              trigger: 'axis',
              axisPointer: {
                  type: 'line',
                  lineStyle: {
                      opacity: 0
                  }
              },
              formatter: function(prams) {
                 return "出警数：" + prams[0].data
              }
          },
          grid: {
              left: '4%',
              right: '0%',
              bottom: '5%',
              top: '16%',
              height: '85%',
              containLabel: true,
              z: 22
          },
          xAxis: [{
              type: 'category',
              gridIndex: 0,
              data: [],
              axisTick: {
                  alignWithLabel: true
              },
              axisLine: {
                  lineStyle: {
                      color: '#0c3b71'
                  }
              },
              axisLabel: {
                  show: true,
                  color: 'rgb(255,255,255)',
                  fontSize: 13
              }
          }],
          yAxis: [{
                  type: 'value',
                  gridIndex: 0,
                  splitLine: {
                      show: false
                  },
                  axisTick: {
                      show: false
                  },
                  axisLine: {
                      lineStyle: {
                          color: '#0c3b71'
                      }
                  },
                  axisLabel: {
                      color: 'rgb(170,170,170)',
                      formatter: '{value}',
                      textStyle: {
                          color: '#2ad1d2'
                      }
                  }
              },
              {
                  type: 'value',
                  gridIndex: 0,
                  splitNumber: 12,
                  splitLine: {
                      show: false
                  },
                  axisLine: {
                      show: false
                  },
                  axisTick: {
                      show: false
                  },
                  axisLabel: {
                      show: false
                  },
                  splitArea: {
                      show: true,
                      areaStyle: {
                          color: ['rgba(250,250,250,0.0)', 'rgba(250,250,250,0.05)']
                      }
                  }
              }
          ],    　
          series: [{
                  name: '出警数',
                  type: 'bar',
                  barWidth: '30%',
                  xAxisIndex: 0,
                  yAxisIndex: 0,
                  label: {
                      normal: {
                          show: true,
                          position: "top",
                          textStyle: {
                              color: "#ffc72b",
                              fontSize: 20
                          }
                      }
                  },
                  itemStyle: {
                      normal: {
                          color: new echarts.graphic.LinearGradient(
                              0, 0, 0, 1, [{
                                      offset: 0,
                                      color: '#00feff'
                                  },
                                  {
                                      offset: 0.5,
                                      color: '#027eff'
                                  },
                                  {
                                      offset: 1,
                                      color: '#0286ff'
                                  }
                              ]
                          )
                      }
                  },
                  data: [],
                  zlevel: 11

              },
              {
                  name: '背景',
                  type: 'bar',
                  barWidth: '50%',
                  xAxisIndex: 0,
                  yAxisIndex: 1,
                  barGap: '-135%',
                  data: [100, 100, 100, 100, 100, 100, 100],
                  itemStyle: {
                      normal: {
                          color: 'rgba(255,255,255,0.1)'
                      }
                  },
                  zlevel: 9
              },

          ]
        },
        chartcontainer:'',
        chartData:[],
        datatimer:''
      }
    },
    watch: {
      chartData(newValue, oldValue) {
        this.chartcontainer.setOption({
          xAxis : [{
                      data :newValue.map((x)=>(x.name))
          }],
          series : [
                {
                  data:newValue.map((x)=>(x.value)).map(Number)
                }
            ]
        })
      }
    },
    methods: {
      seriesDate() {
        var that = this
         axios.get('../../../static/data/cjr.json')
         // axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=top5&m=ns_klny')
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
      this.chartcontainer = echarts.init(document.getElementById('chartH5'))
      // 绘制图表，this.echarts1_option是数据
      this.chartcontainer.setOption(this.echarts1_option)
      //调用异步方法获取数据
      this.seriesDate()
      //设置定时器每十秒自动刷新数据
      this.datatimer=setInterval(this.seriesDate,10000)
    },
    beforeDestroy() {
      clearInterval(this.flashtimer)
    }
 }
</script>

<style scoped>
  #chartH5{
    margin-left: 0.65rem;
  }
</style>
