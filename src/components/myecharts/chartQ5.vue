<template>
  <div id='chartQ5' style="margin-left: 5px;" :style="{ width: blockWidth, height: blockHeight}"></div>
</template>

<script>

//先要导入依赖的实例
import echarts from 'echarts'
import axios from 'axios'
 export default {
    name: 'chartQ5',
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
          grid: {
              left: '0%',
              right: '12%',
              bottom: '5%',
              top: '13%',
              height: '85%',
              containLabel: true,
              z: 22
          },
          legend: {
              show: true,
              x: 700,
              y: 50,
              textStyle: {
                  color: '#fff', // 图例文字颜色
                  fontSize: 12,
              }
          },
          xAxis: {
              type: "category",
              boundaryGap: false,
              data: [],
              axisLabel: {
                  show: true,
                  textStyle: {
                      color: "#fff",
                      fontSize: 14
                  }
              },
              axisLine: {
                  lineStyle: {
                      color: 'transparent',
                      width: 2 //这里是为了突出显示加上的
                  }
              }
          },
          tooltip: {
              show: true,
              trigger: 'item'
          },
          yAxis: [{
              type: 'value',

              axisLabel: {
                  margin: 30,
                  formatter: '{value}',
                  textStyle: {
                      color: '#2ad1d2'
                  }
              },
              axisLine: {
                  lineStyle: {
                      color: 'transparent',
                      width: 2 //这里是为了突出显示加上的
                  }
              },
              axisTick: {
                  show: false,
              },
              splitLine: {
                  show: true,
                  lineStyle: {
                      color: 'rgba(255,255,255,.1)'
                  }
              }
          }],
          series: [
              {
                  name: '提示值',
                  type: 'line',
                  smooth: true,
                  // symbol: "circle", //拐点的形状
                  // symbolSize: 10,
                  symbol: "none",
                  itemStyle: {
                      normal: {
                          lineStyle: {
                              type: 'dashed',
                              //折点的颜色
                              // color: "#00a2e6"
                          },
                          color: "#01F699", //拐点的颜色
                          borderColor: "#01F699" //拐点边框的颜色
                      }
                  },
                  data: []
              },
              {
                  name: '当前出警数',
                  type: 'bar',
                  barWidth: 30,
                  itemStyle: {
                      normal: {
                          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
                              offset: 0,
                              color: 'rgba(18,93,236,.8)'

                          }, {
                              offset: 1,
                              color: 'rgba(3,191,255,.8)'
                          }]),
                      },

                  },
                  data: []
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
                  data:[10,10,10,10,10]
                },
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
         // axios.get('../../../static/data/cjr.json')
         axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=bottom5&m=ns_klny')
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
      this.chartcontainer = echarts.init(document.getElementById('chartQ5'))
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
  #chartQ5{
    width: 80%;
  }
</style>
