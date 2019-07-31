<template>
  <div id="app">
    <Header/>
    <clock id="clock"
    size="150px"
    color="#26c6f0"
    ></clock>
    <Block
    left-title='出警次数前五'
    block-height='305px'
    block-width='380px'
    block-left='15px'
    block-top='-170px'
    >
    <charts3
    block-height='305px'
    block-width='380px'
    ></charts3>
    </Block>
    <Block
    left-title='报修频率前五'
    block-height='285px'
    block-width='380px'
    block-left='15px'
    block-top='-140px'
    >
      <charts2
      block-height='285px'
      block-width='380px'
      ></charts2>
    </Block>
    <Block
    left-title='报修预约状况分布'
    block-height='385px'
    block-width='640px'
    block-left='425px'
    block-top='-725px'
    >
    <ul class="data-box1-data clearfix">
      <li class="data-box1-font1">预约总数</li>
      <li >
        <div style="width: 7rem;">
          <span id="zyy" class="data-box1-panel">50000</span>
          <span class="data-box1-font1">次</span>
        </div>
      </li>
      <li class="data-box1-font1">报修总数</li>
      <li >
        <div style="width: 7rem;">
          <span id="zbx" class="data-box1-panel">50000</span>
          <span class="data-box1-font1" >次</span>
        </div>
      </li>
      <li class="data-box1-font1">今日预约</li>
      <li >
        <div style="width: 7rem;">
          <span id="jyy" class="data-box1-panel">50000</span>
          <span class="data-box1-font1" >次</span>
        </div>
      </li>
      <li class="data-box1-font1">今日报修</li>
      <li >
        <div style="width: 7rem;">
          <span id="jbx" class="data-box1-panel">50000</span>
          <span class="data-box1-font1" >次</span>
        </div>
      </li>
		</ul>
    <charts4
      block-height='365px'
      block-width='620px'
    ></charts4>
    </Block>
    <Block
    left-title='近期预约趋势'
    block-height='220px'
    block-width='640px'
    block-left='425px'
    block-top='-695px'
    >
    <charts5
      block-height='235px'
      block-width='630px'
    >
    </charts5>
    </Block>
    <Block
    left-title='实时维修预约处理率'
    block-height='215px'
    block-width='400px'
    block-left='1095px'
    block-top='-1270px'
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
    left-title='历年预约总数据统计'
    block-height='285px'
    block-width='400px'
    block-left='1095px'
    block-top='-1240px'
    >
    <charts1
    block-height='285px'
    block-width='400px'
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
import Charts4 from './components/myecharts/Charts4'
import Charts5 from './components/myecharts/Charts5'
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
    Charts4,
    Charts5,
    percentbar
  },
  data() {
    return {
      bardata: ['25%','69%','72%']
    }
  },
  mounted:function () {
    let that=this;
    // axios.get('../../../static/data/persent.json')
    axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=bar&m=ns_klny')
     .then(function (response) {
       document.getElementById('jyy').innerText=response.data[0][0]
       document.getElementById('jbx').innerText=response.data[1][0]
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
}
</script>

<style>
#app {
  position: relative;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
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
  left: 0.5rem;
  top: 1.5rem;
  width: 6rem;
}
.data-box1-font1{
  font-size: 1.25rem;
}
.data-box1-panel{
  font-size: 1.25rem;
}
</style>
