<template >
  <div id='myChart1' v-on:click="changeData" :style="{ width: blockWidth, height: blockHeight}">
  </div>
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
            tooltip : {
                trigger: 'axis'
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
            grid: {
                top: 'middle',
                left: '5%',
                right: '8%',
                bottom: '0%',
                height: '70%',
                containLabel: true,
                show:true,
                borderColor:"transparent",
                backgroundColor:'rgba(255,255,255,.1)'
            },
            textStyle: {
                color: 'rgba(38, 198, 248, 1)'
            },
            legend: {
              data: ['预约完成', '预约未完成'],
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
                name: '预约完成',
                type: 'bar',
                "stack": '1',
                label: {
                  normal: {
                    show: true,
                    position: 'insideRight',
                    textStyle: {
                        color: 'rgba(255, 255, 255, 1)',
                        fontSize:'15'
                    },
                  }
                },
                data: []
              },
              {
                name: '预约未完成',
                type: 'bar',
                stack: '1',
                label: {
                  normal: {
                    show: true,
                    position: 'insideRight',
                    textStyle: {
                        color: 'rgba(255, 255, 255, 1)',
                        fontSize:'15'
                    },
                  }
                },
                data: []
              }
            ]
        },
        chartcontainer:'',
        chartData:[],
        datatimer:'',
        timer:0,
        datastatus:[
          ['预约完成','预约未完成'],
          ['报修完成','报修未完成']
        ]
      }
    },
    watch: {
      chartData(newValue, oldValue) {
        this.chartData=newValue
        this.chartData[0][1]=this.chartData[0][1].map((x)=>(x==0?x="":x))
        this.chartData[0][0]=this.chartData[0][0].map((x)=>(x==0?x="":x))
        this.chartData[1][1]=this.chartData[1][1].map((x)=>(x==0?x="":x))
        this.chartData[1][0]=this.chartData[1][0].map((x)=>(x==0?x="":x))
        this.updatachart(this.timer)
      }
    },
    methods: {
      seriesDate() {
        var that = this
         axios.get('../../../static/data/y2.json')
         // axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=bb&m=ns_klny')
          .then(function (response) {
            that.chartData=response.data;
          })
          .catch(function (error) {
            console.log(error)
          });
      },
      changeData(){
        let that=this
        if(that.timer===0){
          that.timer=1
          this.updatachart(1)
        }else{
          that.timer=0
          this.updatachart(0)
        }
      },
      updatachart(a){
        let that=this
        this.chartcontainer.setOption({
          legend: {
            data: that.datastatus[a]
          },
          series : [
                {
                  name:that.datastatus[a][0],
                  data:that.chartData[a][0]
                },
                {
                  name:that.datastatus[a][1],
                  data:that.chartData[a][1]
                }
            ]
        })
      }
    },
    //挂载前初始化echarts实例
    mounted: function () {
      // 基于准备好的dom，初始化echarts实例
      this.chartcontainer = echarts.init(document.getElementById('myChart1'),'light')
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
