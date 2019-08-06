<template>
  <div id='myChart5' style="margin-left: 5px;" :style="{ width: blockWidth, height: blockHeight}"></div>
</template>

<script>

//先要导入依赖的实例
import echarts from 'echarts'
import axios from 'axios'
 export default {
    name: 'Charts5',
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
          symbol:'emptyCircle',
          tooltip : {
              trigger: 'axis'
          },
          textStyle: {
              color: 'rgba(38, 198, 248, 1)'
          },
          grid:{
            top: 'middle',
            left: '3%',
            right: '4%',
            bottom: '0%',
            height: '70%',
            containLabel: true,
            show:true,
            borderColor:"transparent",
            backgroundColor:'rgba(255,255,255,.2)'
          },
          legend: {
              data:['滨湖一','滨湖二','包河区'],
              textStyle: {
                  color: 'rgba(38, 198, 248, 1)'
              }
          },
          toolbox: {
              show : true,
              orient : 'vertical',
              left: '93%',
              top: 'top',
              feature : {
                saveAsImage : {show: true}
              }
          },
          calculable : true,
          xAxis : [
              {
                  type : 'category',
                  boundaryGap : false,
                  data :[]
              }
          ],

          yAxis : [
              {
                  type : 'value'
              }
          ],

          series : [
              {
                  name:'滨湖一',
                  type:'line',
                  smooth:true,
                  itemStyle: {normal: {areaStyle: {type: 'default'}}},
                  data:[],
              },
              {
                  name:'滨湖二',
                  type:'line',
                  smooth:true,
                  itemStyle: {normal: {areaStyle: {type: 'default'}}},
                  data:[],
                 
              },
              {
                  name:'包河区',
                  type:'line',
                  smooth:true,
                  itemStyle: {normal: {areaStyle: {type: 'default'}}},
                  data:[]
              }
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
                      data :newValue[0]
          }],
          series : [
                {
                  data:newValue[1].map(Number)
                },
                {
                  data:newValue[3].map(Number)
                },
                {
                  data:newValue[2].map(Number)
                }
            ]
        })
      }
    },
    methods: {
      seriesDate() {
        var that = this
         // axios.get('../../../static/data/char5.json')
         axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=ss&m=ns_klny')
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
      this.chartcontainer = echarts.init(document.getElementById('myChart5'),'light')
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
</style>
