<template>
  <div id="title">
    <div id="leftblock1" class="caption-title clearfix">
      <i class="title-left"><img src="../../static/images/title-left.png" ></i>
      <p class="title-left-title-font">{{centertitle}}</p>
      <i class="title-left"><img src="../../static/images/title-right.png" ></i>
    </div>
    <div class="title-date" style="height: 40px;">
      <p id="DateTime" style="height: 40px;text-align: center;">{{mydatatime}}</p>
    </div>
  </div>

</template>

<script>
  export default {
    name: 'Header',
    data() {
      return {
        centertitle: '合肥昆仑燃气业务数据'
      };
    },
    computed:{
    mydatatime: function() {
      let timeStamp=new Date();
      return timeStamp.getFullYear() + "年" + (Number(timeStamp.getMonth())+1) + "月" + timeStamp.getDate() +"日";
    }
   }
};

</script>

<style scoped>

</style>
