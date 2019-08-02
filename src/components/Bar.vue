<template>
  <div id='barapp'>
    <div id='bartitle' :style="{color:barcolor}">{{bartext}}</div>
    <div class="lanren" :style="{borderColor:barcolor}">
    	<div id="bar" :style="{width:barwidth,background:barcolor}">
        {{barwidth}}
      </div>
    </div>
  </div>
</template>

<script>
  export default{
    name : 'percentbar',
    props: {
      barwidth: {
        type: String,
        default: '50%'
      },
      barcolor:{
        type: String,
        default: '#27C6F8'
      },
      bartext:{
        type:String,
        default: '默认值'
      }
    },
  }
</script>

<style scoped>
    .lanren,#bar,#barapp,#bartitle{margin:0;
                padding:0;
                list-style:none;
    }
    #barapp{
      margin: 26px auto;
      width: 85%;
    }
    #bartitle{
        height: 100%;
        width: 13%;
        text-align: center;
        line-height: 28px;
        font-size: 16px;
        float: left;
    }
		.lanren{
            width: 85%;
            margin-left: 15%;
            border: 1px solid;
            height: 28px;
            }
		#bar{ display: block;
          float: left;
          height: 100%;
          text-align: center;
          line-height: 28px;
          font-size: 18px;
          color:#fff;
    }

</style>
