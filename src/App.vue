<template>
  <div id="app">
    <Header/>
    <clock id="clock"
    size="150px"
    color="#26c6f0"
    ></clock>
    <Block
    left-title='历年统计数据左一'
    block-height='285px'
    block-width='400px'
    block-left='15px'
    block-top='-150px'
    ></Block>
    <Block
    left-title='历年统计数据左二'
    block-height='285px'
    block-width='400px'
    block-left='15px'
    block-top='-120px'
    ></Block>
    <Block
    left-title='历年统计数据中一'
    block-height='385px'
    block-width='620px'
    block-left='445px'
    block-top='-705px'
    ></Block>
    <Block
    left-title='历年统计数据中二'
    block-height='170px'
    block-width='620px'
    block-left='445px'
    block-top='-675px'
    ></Block>
    <Block
    left-title='历年统计数据'
    block-height='185px'
    block-width='400px'
    block-left='1095px'
    block-top='-1210px'
    ></Block>
    <Block
    left-title='历年统计数据右二'
    block-height='305px'
    block-width='400px'
    block-left='1095px'
    block-top='-1180px'
    ></Block>
  </div>
</template>


<script>
import Header from './components/Header'
import Block from './components/Block'
import Clock from 'vue-clock2';
export default {
  name: 'App',
  components: {
    Header,
    Block,
    Clock
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
  margin-top: 10px;
}
#app > #clock{
  right: -535px;
  top: -90px;
}
</style>
