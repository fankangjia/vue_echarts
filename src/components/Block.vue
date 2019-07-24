<template>
  <div class="data-box1 clearfix" :style="{ width: blockWidth, height: blockHeight, left:blockLeft,top:blockTop}">
    <i class="topL"></i>
    <i class="topR"></i>
    <i class="bottomL"></i>
    <i class="bottomR"></i>
    <div class="data-title">
      <b class="data-title-left">[</b>
      <span>{{leftTitle}}</span>
      <b class="data-title-right">]</b>
    </div>
    <charts1
    :block-height='blockHeight'
    :block-width='blockWidth'
    ></charts1>
  </div>

</template>

<script>
 import Charts1 from './myecharts/Charts1'
 export default {
   name: 'Block',
   components: {
     Charts1
   },
   // data () {
   //   return {
   //     lefttitle: '统计数据'
   //   }
   // },
   props: {
     leftTitle: {
       type: String,
       default: '统计数据'
     },
     blockWidth: {
       type: String,
       default: '200px'
     },
     blockHeight: {
       type: String,
       default: '200px'
     },
     blockLeft: {
       type: String,
       default: '50px'
     },
     blockTop: {
       type: String,
       default: '35px'
     },
   },
 }
</script>

<style scoped>

</style>
