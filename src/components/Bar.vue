<template>
  <div class="lanren" :style="{borderColor:barcolor}">
		<div id="bar" :style="{width:barwidth,background:barcolor}">
      <!-- {{bartext}} -->{{barwidth}}
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
    .lanren,#bar{margin:0;
                padding:0;
                list-style:none;
    }
		.lanren{width: 80%;
            border: 1px solid;
            height: 25px;
            margin: 20px auto;
            }
		#bar{ display: block;
          float: left;
          height: 100%;
          text-align: center;
          line-height: 25px;
          font-size: 15.5px;;
          color:#fff;
    }
</style>
