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
    <charts4
      block-height='365px'
      block-width='620px'
    ></charts4>
    </Block>
    <Block
    left-title='报修预约状况走势'
    block-height='220px'
    block-width='640px'
    block-left='425px'
    block-top='-695px'
    ></Block>
    <Block
    left-title='今日维修预约完成概况'
    block-height='215px'
    block-width='400px'
    block-left='1095px'
    block-top='-1270px'
    >
      <percentbar
        barwidth='25%'
        bartext='维修完成'
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
import percentbar from './components/Bar'
import Clock from 'vue-clock2'
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
    percentbar
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
</style>
