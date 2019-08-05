<template>
  <div id="app">
    <Header/>
    <clock id="clock"
    size="150px"
    color="#26c6f0"
    >
    </clock>
    <Block
    left-title='出警次数统计'
    block-height='424px'
    block-width='350px'
    block-left='15px'
    block-top='-190px'
    >
    <div class="total-mn" id="total-mn1">
      <span id="topleft">前五名</span>
    </div>
    <div id="box1_top">
      <div id="box2" class="box-echart box-bottom">
        <chartH5
        block-height='180px'
        block-width='320px'
        ></chartH5>
    </div>
    </div>
    <div class="total-mn" id="total-mn2">
      <span id="topleft">后五名</span>
    </div>
    <div id="box1_bottom">
      <div id="box4" class="box-echart">
        <chartQ5
        block-height='150px'
        block-width='320px'
        ></chartQ5>
      </div>
    </div>
    </Block>
    <Block
    left-title='报修频率前五'
    block-height='285px'
    block-width='350px'
    block-left='15px'
    block-top='-160px'
    >
      <charts2
      block-height='285px'
      block-width='350px'
      ></charts2>
    </Block>
    <Block
    left-title='报修预约热力图'
    block-height='435px'
    block-width='720px'
    block-left='385px'
    block-top='-845px'
    >
    <ul class="data-box1-data clearfix">
      <li class="data-box1-font1">预约总数</li>
      <li >
        <div style="width: 4.8rem;">
          <span id="zyy" class="data-box1-panel">50000</span>
          <span class="data-box1-font1">次</span>
        </div>
      </li>
      <li class="data-box1-font1">报修总数</li>
      <li >
        <div style="width: 4.8rem;">
          <span id="zbx" class="data-box1-panel">50000</span>
          <span class="data-box1-font1" >次</span>
        </div>
      </li>
      <li class="data-box1-font1" style="margin-right: 0px;">今日预约</li>
      <li >
        <div style="width: 8rem;">
          <span id="jyy" class="data-box1-panel" >50</span>
          <span class="data-box1-font1">次&nbsp;&nbsp;报修</span>
          <span id="jbx" class="data-box1-panel">50</span>
          <span class="data-box1-font1" >次</span>
        </div>
      </li>
		</ul>
    <chartmap
      block-height='415px'
      block-width='700px'
    ></chartmap>
    </Block>
    <Block
    left-title='近期预约趋势'
    block-height='220px'
    block-width='640px'
    block-left='425px'
    block-top='-815px'
    >
    <charts5
      block-height='220px'
      block-width='630px'
    >
    </charts5>
    </Block>
    <Block
    left-title='实时维修预约处理率'
    block-height='215px'
    block-width='370px'
    block-left='1125px'
    block-top='-1440px'
    >
      <percentbar
        :barwidth='bardata[0]'
        bartext='预约:'
      >
      </percentbar>
      <percentbar
        :barwidth='bardata[1]'
        bartext='维修:'
        barcolor="#ed5a65"
      >
      </percentbar>

      <percentbar
        :barwidth='bardata[2]'
        bartext='完成:'
        barcolor="#f9d367"
      >
      </percentbar>
    </Block>
    <Block
    left-title='实时维修预约统计'
    block-height='285px'
    block-width='370px'
    block-left='1125px'
    block-top='-1410px'
    >
    <charts1
    block-height='285px'
    block-width='370px'
    ></charts1>
    </Block>
  </div>
</template>


<script>
import Header from './components/Header'
import Block from './components/Block'
import Charts1 from './components/myecharts/Charts1'
import Charts2 from './components/myecharts/Charts2'
import Charts3 from './components/myecharts/Charts3'
import chartmap from './components/myecharts/chartmap'
import Charts5 from './components/myecharts/Charts5'
import chartQ5 from './components/myecharts/chartQ5'
import chartH5 from './components/myecharts/chartH5'
import percentbar from './components/Bar'
import Clock from 'vue-clock2'
import axios from 'axios'
export default {
  name: 'App',
  components: {
    Header,
    Block,
    Clock,
    Charts1,
    Charts2,
    Charts3,
    chartmap,
    Charts5,
    chartQ5,
    chartH5,
    percentbar
  },
  data() {
    return {
      bardata: ['25%','69%','72%'],
      datatimer:''
    }
  },
  methods: {
    getData(){
      let that=this;
      axios.get('../../../static/data/persent.json')
      // axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=bar&m=ns_klny')
       .then(function (response) {
         document.getElementById('jyy').innerText=response.data[0][0]
         document.getElementById('jbx').innerText=response.data[1][0]
         document.getElementById('zyy').innerText=response.data[2][0]
         document.getElementById('zbx').innerText=response.data[2][1]
         response.data[0]=response.data[0].map(Number)
         response.data[1]=response.data[1].map(Number)
         that.bardata=[]
         if(response.data[0][0]<=0){
           that.bardata[0]='0%'
         }else{
           if(response.data[0][1]<=0){
             that.bardata[0]='0%'
           }else{
             that.bardata[0]=((response.data[0][1]/response.data[0][0])*100).toFixed(1).toString()+'%'
           }
         }


         if(response.data[1][0]<=0){
           that.bardata[1]='0%'
           that.bardata[2]='0%'
         }else{
           if(response.data[1][2]<=0&&response.data[1][1]<=0){
             that.bardata[1]='0%'
             that.bardata[2]='0%'
           }else{
             that.bardata[1]=(((response.data[1][1]+response.data[1][2])/response.data[1][0])*100).toFixed(1).toString()+'%'
             that.bardata[0]=((response.data[1][2]/response.data[1][0])*100).toFixed(1).toString()+'%'
           }
         }
              })
       .catch(function (error) {
         console.log(error)
       });
    }
  },
  mounted:function () {
    //获取数据
    this.getData()
    //设置定时器，每10秒刷新一次
    this.datatimer=setInterval(this.getData,10000)
  },
  beforeDestroy() {
    clearInterval(this.flashtimer)
  }
}
</script>

<style>
#app {
  position: relative;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 850px;
  margin-top: 10px;
}
#app > #clock{
  right: -535px;
  top: -90px;
}
.data-box1-data{
  display: inline-block;
  position: absolute;
  left: -0.7rem;
  top: 0.2rem;
  width: 11.9rem;
}
.data-box1-font1{
  font-size: 0.75rem;
}
#topleft{
  font-size: 1.1rem;
  color: #83c7e3;
  position: absolute;
  left: 0.7rem;
}
.data-box1-panel{
  font-size: 0.75rem;
}
#box1_top{
  margin-top: 40px;
  height: 188px;
}
#box1_bottom{
  margin-top: 20px;
  height: 191px;
}
#box2{
  height: 100%;
}
#box4{
  margin-left: 0.65rem;
}
#topleft{
  margin-top: 0.3rem;
}
</style>
