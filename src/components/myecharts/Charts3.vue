<template>
  <div id='myChart3' :style="{ width: blockWidth, height: blockHeight}"></div>
</template>

<script>

//先要导入依赖的实例
import echarts from 'echarts'
import axios from 'axios'
 export default {
    name: 'Charts3',
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
            formatter: "{b} : {c} ({d}%)"
        },
        toolbox: {
            show : true,
            orient : 'vertical',
            left: '80%',
            top: 'top',
            feature : {
              saveAsImage : {show: true}
            }
        },
        visualMap: {
            show: false,
            min: 500,
            max: 600,
            inRange: {
                //colorLightness: [0, 1]
            }
        },
        series: [{
            name: '访问来源',
            type: 'pie',
            radius: '50%',
            center: ['50%', '50%'],
            color: ['rgb(131,249,103)', '#FBFE27', '#FE5050', '#1DB7E5'], //'#FBFE27','rgb(11,228,96)','#FE5050'
            data: [],
            roseType: 'radius',

            label: {
                normal: {
                    formatter: ['{c|{c}次}', '{b|{b}}'].join('\n'),
                    rich: {
                        c: {
                            color: 'rgb(241,246,104)',
                            fontSize: 16,
                            fontWeight:'bold',
                            lineHeight: 5
                        },
                        b: {
                            color: 'rgb(98,137,169)',
                            fontSize: 15,
                            height: 40
                        },
                    },
                }
            },
            labelLine: {
                normal: {
                    lineStyle: {
                        color: 'rgb(98,137,169)',
                    },
                    smooth: 0.2,
                    length: 10,
                    length2: 20,

                }
            },
            itemStyle: {
                normal: {
                    shadowColor: 'rgba(0, 0, 0, 0.8)',
                    shadowBlur: 50,
                }
            }
        }]
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
                  data:newValue
                }
            ]
        })
      }
    },
    methods: {
      seriesDate() {
        var that = this
         // axios.get('../../../static/data/cjr.json')
         axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=cc&m=ns_klny')
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
      this.chartcontainer = echarts.init(document.getElementById('myChart3'),'light')
      // 绘制图表，this.echarts1_option是数据
      this.chartcontainer.setOption(this.echarts1_option)
      //调用异步方法获取数据
      this.seriesDate()
    },
 }
</script>

<style scoped>
</style>
