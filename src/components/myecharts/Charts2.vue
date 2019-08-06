<template>
  <div style="position: relative;">
    <div id='xmyear' :style="{ width: blockWidth, height: blockHeight}"></div>
    <ul class="boxall">
      <li class="boxset">
        <div id="monthbx" class="boxclick"><a class="boxword" style="color:#83c7e3;">月</a></div>
      </li>
      <li class="boxset">
        <div id="weekbx" class="boxclick"><a class="boxword" style="color:#83c7e3;">周</a></div>
      </li>
      <li class="boxset">
        <div id="daybx" class="boxclick"><a class="boxword" style="color:#83c7e3;">日</a></div>
      </li>
    </ul>
  </div>
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
          toolbox: {
              show : true,
              orient : 'vertical',
              left: '80%',
              top: 'top',
              feature : {
                saveAsImage : {show: true}
              }
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
                  radius: ['55%', '80%'],
                   center: ['53%', '50%'],
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
        chartData:[],
        flashtimer:'',
        datatimer:'',
        timer:0
      }
    },
    watch: {
      chartData(newValue, oldValue) {
        if(oldValue.length===0){
          this.setChardata()
        }
        this.chartData=newValue
        //把数据设置到图表上去
        this.changeData()
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
            console.log('获取数据失败')
          });
      },
      flashdiv(){
        if(this.timer==0){
          document.getElementById('monthbx').classList.remove('choseon')
          document.getElementById('weekbx').classList.add('choseon')
          this.timer++
          this.changeData()
        }else if(this.timer==1){
          document.getElementById('weekbx').classList.remove('choseon')
          document.getElementById('daybx').classList.add('choseon')
          this.timer++
          this.changeData()
        }else{
          document.getElementById('daybx').classList.remove('choseon')
          document.getElementById('monthbx').classList.add('choseon')
          this.timer=0
          this.changeData()
        }
      },
      changeData(){
        let that=this
        this.chartcontainer.setOption({
          legend: {
              data:that.chartData[that.timer].map((x)=>x['name'])
          },
          series : [{
                data:that.chartData[that.timer]
          }]
        })
      },
      setChoseon(chose){
        document.getElementById('monthbx').classList.remove('choseon')
        document.getElementById('weekbx').classList.remove('choseon')
        document.getElementById('daybx').classList.remove('choseon')
        document.getElementById(chose['name']).classList.add('choseon')
        this.timer=chose['id']
        this.changeData()
      },
      setChardata(){

        this.changeData()
        clearInterval(this.flashtimer)
        this.flashtimer=setInterval(this.flashdiv,5000)
      }
    },
    //挂载前初始化echarts实例
    mounted: function () {
      var self = this
      // 基于准备好的dom，初始化echarts实例
      this.chartcontainer = echarts.init(document.getElementById('xmyear'),'light')
      // 绘制图表，this.echarts1_option是数据
      this.chartcontainer.setOption(this.echarts1_option)
      //调用异步方法获取数据,并设置10秒刷新一次
      this.seriesDate()
      document.getElementById('monthbx').classList.add('choseon')
      this.datatimer=setInterval(this.seriesDate,10000)

    },
    beforeDestroy() {
      clearInterval(this.flashtimer);
      clearInterval(this.datatimer);
    }
 }
</script>

<style scoped>
.boxall {
  /* ul */
  position: absolute;
  right: 0.2rem;
  top: 3.5rem;
  width: 2rem;
}
.boxset {
  /* li */
  list-style: none;
  height: 2rem;
}
.boxclick {
  /* div */
  width: 100%;
  height: 100%;
  margin-top: 0.65rem;
  border-radius: 25px;
}
.boxword {
  /* a */
  font-size: 1rem;
  line-height: 2rem;
  text-decoration: none;
}
.choseon{
  background-color: rgba(255, 255, 255,.3);
}
</style>
