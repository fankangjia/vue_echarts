<template>
  <div id="app">
    <Header/>
    <clock :time="time"
    style="right: -590px;top: -90px;"
    size="165px"
    color="#26c6f0"
    ></clock>
    <Block
    left-title='统计数据'
    block-height='650px'
    block-width='650px'
    block-left='50px'
    block-top='-145px'
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
};
</style>
