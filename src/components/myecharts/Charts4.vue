<template>
  <div id='myChart4' style="margin-left: 10px;" :style="{ width: blockWidth, height: blockHeight}"></div>
</template>

<script>

//先要导入依赖的实例
import echarts from 'echarts'
import axios from 'axios'
 export default {
    name: 'Charts4',
    props: {
      blockWidth: {
        type: String,
        default: '200px'
      },
      blockHeight: {
        type: String,
        default: '200px'
      },
    },
    data () {
      return {
        echarts1_option:{
          tooltip: {
              trigger: 'item',
              //formatter: '{b0}<br>{a0}、{a1}<br/>{c0} (次)'
          },
          renderer: 'canvas',
          toolbox: {
              show : true,
              orient : 'vertical',
              left: '90%',
              top: 'top',
              feature : {
                saveAsImage : {show: true}
              }
          },
            legend:{

                orient: 'vertical',
                x: 'left',
                y: 'bottom',
                data: [
                  '预约',
                  '报修',

                ],
                textStyle: {
                  color: '#ccc'
                }

            },
            visualMap: {
                  min: 500,
                  max: 50000,
                  text:['High','Low'],
                  left: 'right',
                  realtime: false,
                  calculable: true,
                  // inRange: {
                  //     color: ['#313695','#4575b4', '#74add1','#abd9e9','#e0f3f8']
                  // }
               inRange:{
                  color: ['orangered','yellow','lightskyblue']

              },
	          textStyle: {
	          	color: '#fff'
	          }
	        },
          series: [
              {
                name: '预约',
                  type: 'map',
                  mapType: 'xicheng',
                  //aspectScale: 0.85,  //地图长度比
                  label: {
                      normal: {
                          show: true
                      },
                      emphasis: {
                          show: true
                      }
                  },
                  data: [],
                  },
                  {
                    name: '报修',
                      type: 'map',
                      mapType: 'xicheng',
                      //aspectScale: 0.85,  //地图长度比
                      label: {
                          normal: {
                              show: true

                          },
                          emphasis: {
                              show: true
                          }
                      },
                      data: []
                  } ,
          ]
        },
        chartcontainer:'',
        chartData:[]
      }
    },
    watch: {
      chartData(newValue, oldValue) {
        this.chartcontainer.setOption({
          series : [
                {
                  data:newValue[0]
                },
                {
                  data:newValue[1]
                }
            ]
        })
      }
    },
    methods: {
      seriesDate() {
        var that = this
         axios.get('../../../static/data/ditu.json')
         // axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=bb&m=ns_klny')
          .then(function (response) {
            that.chartData=response.data
            document.getElementById('zyy').innerText=response.data[0].map((x)=>(Number(x.value))).reduce((x,y)=>(x+y)).toString()
            document.getElementById('zbx').innerText=response.data[1].map((x)=>(Number(x.value))).reduce((x,y)=>(x+y)).toString()
          })
          .catch(function (error) {
            console.log(error)
            console.log('数据读取失败')
          });
      }
    },
    //挂载前初始化echarts实例
    mounted: function () {


      //导入地图数据
      echarts.registerMap('bh',
        {
          "type": "FeatureCollection",
          "features": [
            {
              "type": "Feature",
              "properties": {
                "name": "滨湖一所"
              },
              "geometry": {
                "type": "Polygon",
                "coordinates": [
                  [
                    [
                      117.30802059173583,
                      31.796910936605013
                    ],
                    [
                      117.30475902557372,
                      31.801725465871957
                    ],
                    [
                      117.29772090911865,
                      31.79472243129337
                    ],
                    [
                      117.30385780334473,
                      31.789688672420066
                    ],
                    [
                      117.30656147003172,
                      31.790965376340232
                    ],
                    [
                      117.30943679809572,
                      31.79246092136411
                    ],
                    [
                      117.30802059173583,
                      31.796910936605013
                    ]
                  ]
                ]
              }
            },
            {
              "type": "Feature",
              "properties": {
                "name": "滨湖二所"
              },
              "geometry": {
                "type": "Polygon",
                "coordinates": [
                  [
                    [
                      117.30475902557372,
                      31.801688993108318
                    ],
                    [
                      117.30797767639159,
                      31.79687446194113
                    ],
                    [
                      117.30943679809572,
                      31.79246092136411
                    ],
                    [
                      117.31312751770018,
                      31.794284724012485
                    ],
                    [
                      117.31501579284668,
                      31.795233087167897
                    ],
                    [
                      117.31681823730469,
                      31.796108490674488
                    ],
                    [
                      117.3124837875366,
                      31.804205580024366
                    ],
                    [
                      117.30475902557372,
                      31.801688993108318
                    ]
                  ]
                ]
              }
            },
            {
              "type": "Feature",
              "properties": {
                "name": "包河服务所"
              },
              "geometry": {
                "type": "Polygon",
                "coordinates": [
                  [
                    [
                      117.30385780334473,
                      31.789688672420066
                    ],
                    [
                      117.31038093566895,
                      31.78458168042431
                    ],
                    [
                      117.31904983520506,
                      31.793555207271424
                    ],
                    [
                      117.31681823730469,
                      31.796108490674488
                    ],
                    [
                      117.30385780334473,
                      31.789688672420066
                    ]
                  ]
                ]
              }
            }
          ]
        }
      ,{})
      // 基于准备好的dom，初始化echarts实例
      this.chartcontainer = echarts.init(document.getElementById('myChart4'))
      // 绘制图表，this.echarts1_option是数据
      this.chartcontainer.setOption(this.echarts1_option)
      //异步方法获取数据
      this.seriesDate()

    },
 }
</script>

<style scoped>
</style>
