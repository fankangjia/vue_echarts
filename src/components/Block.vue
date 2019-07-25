<template>
  <div class="data-box1 clearfix" :style="{ width: blockWidth, height: blockHeight, left:blockLeft,top:blockTop}">
    <i class="topL"></i>
    <i class="topR"></i>
    <i class="bottomL"></i>
    <i class="bottomR"></i>
    <div class="data-title" :style="{width:mytextwidth}"><!-- 标题最外层 -->
      <b class="data-title-left">[</b>
      <span id="chartitle">{{leftTitle}}</span>
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
     }
   },
   data () {
     return {
       mytextwidth: function(){
         return this.leftTitle.length*30+30+'px'
       },
       leftTitle:''
     }
   },
   watch:{
     leftTitle:function(newval,oldval){
       this.mytextwidth=this.leftTitle.length*25+30+'px'
      //  let len=document.getElementsByClassName('data-title')[0].offsetWidth
      //  -document.getElementById('chartitle').offsetWidth
      //  -document.getElementsByClassName('data-title-left')[0].offsetWidth*2
      //  let chartitle=document.getElementsByClassName('data-title')[0]
      //  chartitle.style.marginLeft=len/2+'px'
      //  chartitle.style.marginRight=len/2+'px'
     }
   },
  created: function () {
    // `this` 指向 vm 实例
    this.mytextwidth=this.leftTitle.length*25+30+'px'
  }
 }
</script>

<style scoped>

</style>
