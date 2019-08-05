<template>
  <div id='chartmap' style="margin-left: 5px;" :style="{ width: blockWidth, height: blockHeight}"></div>
</template>
<!-- 几乎已经完成，但是发现数据不对！！！！ -->
<script>

//先要导入依赖的实例
import echarts from 'echarts'
import axios from 'axios'
 export default {
    name: 'chartmap',
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
          title: {
              top:20,
              subtext: '',
              x: 'center',
              textStyle: {
                  color: '#ccc'
              }
          },
          tooltip: {
              trigger: 'item',
              formatter: function (params) {
                if(typeof(params.value)[2] == "undefined"){
                  return params.name + ' : ' + params.value;
                }else{
                  return params.name + ' : ' + params.value[2];
                }
              }
          },
          dataRange: {
            min : 0,
            max : 500,
            calculable : true,
            color: ['#FF0000 ','#FFFF00 ','#00FF00'],
            //show:false,
            textStyle:{
              color:'red'
            }
          },
          geo: {
              show: true,
              map: 'bh',
              label: {
                  normal: {
                      show: false
                  },
                  emphasis: {
                      show: false,
                  }
              },

              roam: true,
              itemStyle: {
                  normal: {
                      areaColor: 'green',
                      borderColor: '#3fdaff',
                      borderWidth: 2,
                      shadowColor: 'rgba(63, 218, 255, 0.5)',
                      shadowBlur: 30
                  },
                  emphasis: {
                      areaColor: '#2B91B7',
                  }
              }
          },
          series : [
          {
                name: 'light',
                type: 'scatter',
                coordinateSystem: 'geo',
                data: [],

                label: {
                    normal: {
                        formatter: '   {b}',
                        position: 'right',
                        show: true
                    },
                    emphasis: {
                        show: true
                    }
                }
            },
            {

                type: 'effectScatter',
                coordinateSystem: 'geo',
                data: [],
                symbolSize: function (val) {
                    return val[2] / 10;

                },
                showEffectOn: 'render',
                rippleEffect: {
                    brushType: 'stroke'
                },
                hoverAnimation: true,
                itemStyle: {
                    normal: {
                      color:'orange',
                        shadowBlur: 5,
                        shadowColor: '#05C3F9'
                    }
                },
                zlevel: 1
            }
          ]
        },
        chartcontainer:'',
        chartData:[],
        datatimer:'',
        geoCoordMap:{ "滨湖一所":[
                            117.3024845123291,
                            31.797859272813007
                          ],
                       "滨湖二所": [
                            117.3288345336914,
                            31.765245512758554
                          ],
                       "包河服务所":   [
                            117.35733032226561,
                            31.733715247602944
                          ]
                      }
      }
    },
    watch: {
      chartData(newValue, oldValue) {
        let that=this
        this.chartcontainer.setOption({
          series : [
            {
                data: that.convertData(newValue),
            },
            {
                data: that.convertData(newValue),
            }
          ]
        })
      }
    },
    methods: {
      seriesDate() {
        var that = this
         axios.get('../../../static/data/ditu.json')
         // axios.get('http://b.fankangjia.top/web/index.php?c=site&a=entry&do=ss&m=ns_klny')
          .then(function (response) {
            that.chartData=response.data;
          })
          .catch(function (error) {
            console.log(error)
          });
      },
      convertData(data) {
         var res = [];
          for (var i = 0; i < data.length; i++) {
              var geoCoord = this.geoCoordMap[data[i].name];
              if (geoCoord) {
                  res.push({
                      name: data[i].name,
                      value: geoCoord.concat(data[i].value)
                  });
              }
          }
          return res;
      }
    },
    //挂载前初始化echarts实例
    mounted: function () {
      //导入地图数据
      echarts.registerMap('bh',
        {
          "features": [
              {
                  "type": "Feature",
                  "properties": {
                      "adcode": 340111,
                      "name": "包河区",
                      "center": [
                          117.285751,
                          31.82956
                      ],
                      "centroid": [
                          117.327426,
                          31.739256
                      ],
                      "childrenNum": 0,
                      "level": "district",
                      "subFeatureIndex": 3,
                      "acroutes": [
                          100000,
                          340000,
                          340100
                      ]
                  },
                  "geometry": {
                      "type": "MultiPolygon",
                      "coordinates": [
                          [
                              [
                                  [
                                      117.226789,
                                      31.718449
                                  ],
                                  [
                                      117.227149,
                                      31.719036
                                  ],
                                  [
                                      117.246694,
                                      31.730868
                                  ],
                                  [
                                      117.259154,
                                      31.735372
                                  ],
                                  [
                                      117.26503,
                                      31.743079
                                  ],
                                  [
                                      117.274276,
                                      31.75291
                                  ],
                                  [
                                      117.276217,
                                      31.756763
                                  ],
                                  [
                                      117.276256,
                                      31.769574
                                  ],
                                  [
                                      117.277722,
                                      31.777389
                                  ],
                                  [
                                      117.27506,
                                      31.779973
                                  ],
                                  [
                                      117.27191,
                                      31.780892
                                  ],
                                  [
                                      117.272013,
                                      31.786091
                                  ],
                                  [
                                      117.269827,
                                      31.789974
                                  ],
                                  [
                                      117.268297,
                                      31.791211
                                  ],
                                  [
                                      117.26278,
                                      31.791734
                                  ],
                                  [
                                      117.253509,
                                      31.79064
                                  ],
                                  [
                                      117.249047,
                                      31.787629
                                  ],
                                  [
                                      117.246656,
                                      31.788738
                                  ],
                                  [
                                      117.239133,
                                      31.796266
                                  ],
                                  [
                                      117.239841,
                                      31.798343
                                  ],
                                  [
                                      117.247993,
                                      31.808818
                                  ],
                                  [
                                      117.255477,
                                      31.817533
                                  ],
                                  [
                                      117.263385,
                                      31.827942
                                  ],
                                  [
                                      117.266213,
                                      31.837891
                                  ],
                                  [
                                      117.276153,
                                      31.851878
                                  ],
                                  [
                                      117.276616,
                                      31.8537
                                  ],
                                  [
                                      117.277748,
                                      31.853557
                                  ],
                                  [
                                      117.291661,
                                      31.856725
                                  ],
                                  [
                                      117.298398,
                                      31.860146
                                  ],
                                  [
                                      117.300019,
                                      31.862727
                                  ],
                                  [
                                      117.304018,
                                      31.861349
                                  ],
                                  [
                                      117.305458,
                                      31.859275
                                  ],
                                  [
                                      117.304571,
                                      31.854492
                                  ],
                                  [
                                      117.305535,
                                      31.852337
                                  ],
                                  [
                                      117.311244,
                                      31.848362
                                  ],
                                  [
                                      117.31676,
                                      31.84681
                                  ],
                                  [
                                      117.321364,
                                      31.843657
                                  ],
                                  [
                                      117.323035,
                                      31.843721
                                  ],
                                  [
                                      117.325633,
                                      31.845907
                                  ],
                                  [
                                      117.327446,
                                      31.846192
                                  ],
                                  [
                                      117.331355,
                                      31.84323
                                  ],
                                  [
                                      117.333901,
                                      31.842754
                                  ],
                                  [
                                      117.338131,
                                      31.839824
                                  ],
                                  [
                                      117.340163,
                                      31.834739
                                  ],
                                  [
                                      117.346219,
                                      31.8326
                                  ],
                                  [
                                      117.35027,
                                      31.829954
                                  ],
                                  [
                                      117.352083,
                                      31.827784
                                  ],
                                  [
                                      117.356583,
                                      31.827911
                                  ],
                                  [
                                      117.361032,
                                      31.82452
                                  ],
                                  [
                                      117.363501,
                                      31.824346
                                  ],
                                  [
                                      117.367963,
                                      31.825851
                                  ],
                                  [
                                      117.370162,
                                      31.825693
                                  ],
                                  [
                                      117.372335,
                                      31.823918
                                  ],
                                  [
                                      117.372438,
                                      31.819149
                                  ],
                                  [
                                      117.3751,
                                      31.816376
                                  ],
                                  [
                                      117.37996,
                                      31.816265
                                  ],
                                  [
                                      117.381053,
                                      31.813302
                                  ],
                                  [
                                      117.379742,
                                      31.807803
                                  ],
                                  [
                                      117.38023,
                                      31.804888
                                  ],
                                  [
                                      117.381889,
                                      31.8024
                                  ],
                                  [
                                      117.383985,
                                      31.80194
                                  ],
                                  [
                                      117.390273,
                                      31.802384
                                  ],
                                  [
                                      117.395069,
                                      31.800308
                                  ],
                                  [
                                      117.397705,
                                      31.798105
                                  ],
                                  [
                                      117.402566,
                                      31.796171
                                  ],
                                  [
                                      117.405896,
                                      31.795648
                                  ],
                                  [
                                      117.410628,
                                      31.792146
                                  ],
                                  [
                                      117.413997,
                                      31.786899
                                  ],
                                  [
                                      117.419912,
                                      31.786028
                                  ],
                                  [
                                      117.423551,
                                      31.784744
                                  ],
                                  [
                                      117.427575,
                                      31.77815
                                  ],
                                  [
                                      117.427794,
                                      31.774044
                                  ],
                                  [
                                      117.432063,
                                      31.770477
                                  ],
                                  [
                                      117.431883,
                                      31.763343
                                  ],
                                  [
                                      117.429569,
                                      31.759776
                                  ],
                                  [
                                      117.425364,
                                      31.75754
                                  ],
                                  [
                                      117.419333,
                                      31.755289
                                  ],
                                  [
                                      117.416941,
                                      31.75383
                                  ],
                                  [
                                      117.414305,
                                      31.747916
                                  ],
                                  [
                                      117.408275,
                                      31.739464
                                  ],
                                  [
                                      117.404224,
                                      31.727934
                                  ],
                                  [
                                      117.400598,
                                      31.722367
                                  ],
                                  [
                                      117.399981,
                                      31.720416
                                  ],
                                  [
                                      117.401408,
                                      31.717703
                                  ],
                                  [
                                      117.407439,
                                      31.71461
                                  ],
                                  [
                                      117.409265,
                                      31.71258
                                  ],
                                  [
                                      117.413084,
                                      31.701761
                                  ],
                                  [
                                      117.412634,
                                      31.700206
                                  ],
                                  [
                                      117.408648,
                                      31.69386
                                  ],
                                  [
                                      117.408982,
                                      31.691115
                                  ],
                                  [
                                      117.408789,
                                      31.678849
                                  ],
                                  [
                                      117.409213,
                                      31.666883
                                  ],
                                  [
                                      117.408943,
                                      31.659948
                                  ],
                                  [
                                      117.409959,
                                      31.652964
                                  ],
                                  [
                                      117.410615,
                                      31.636915
                                  ],
                                  [
                                      117.299286,
                                      31.656408
                                  ],
                                  [
                                      117.296071,
                                      31.658471
                                  ],
                                  [
                                      117.296804,
                                      31.661995
                                  ],
                                  [
                                      117.294708,
                                      31.663725
                                  ],
                                  [
                                      117.289745,
                                      31.662551
                                  ],
                                  [
                                      117.285887,
                                      31.660471
                                  ],
                                  [
                                      117.280499,
                                      31.658535
                                  ],
                                  [
                                      117.279098,
                                      31.65636
                                  ],
                                  [
                                      117.278956,
                                      31.647646
                                  ],
                                  [
                                      117.276102,
                                      31.644995
                                  ],
                                  [
                                      117.268747,
                                      31.646281
                                  ],
                                  [
                                      117.265378,
                                      31.645757
                                  ],
                                  [
                                      117.260118,
                                      31.648725
                                  ],
                                  [
                                      117.254088,
                                      31.650884
                                  ],
                                  [
                                      117.252069,
                                      31.653741
                                  ],
                                  [
                                      117.25185,
                                      31.65744
                                  ],
                                  [
                                      117.249356,
                                      31.662836
                                  ],
                                  [
                                      117.248713,
                                      31.666772
                                  ],
                                  [
                                      117.246025,
                                      31.667772
                                  ],
                                  [
                                      117.241949,
                                      31.667788
                                  ],
                                  [
                                      117.239468,
                                      31.672644
                                  ],
                                  [
                                      117.232563,
                                      31.674453
                                  ],
                                  [
                                      117.226879,
                                      31.6795
                                  ],
                                  [
                                      117.225413,
                                      31.682451
                                  ],
                                  [
                                      117.224205,
                                      31.68783
                                  ],
                                  [
                                      117.220386,
                                      31.693796
                                  ],
                                  [
                                      117.217904,
                                      31.699651
                                  ],
                                  [
                                      117.218367,
                                      31.703934
                                  ],
                                  [
                                      117.221581,
                                      31.705854
                                  ],
                                  [
                                      117.226288,
                                      31.707139
                                  ],
                                  [
                                      117.227573,
                                      31.710565
                                  ],
                                  [
                                      117.226789,
                                      31.718449
                                  ]
                              ]
                          ]
                      ]
                  }
              }
          ]
      }
      ,{})
      // 基于准备好的dom，初始化echarts实例
      this.chartcontainer = echarts.init(document.getElementById('chartmap'),'light')
      // 绘制图表，this.echarts1_option是数据
      this.chartcontainer.setOption(this.echarts1_option)
      //调用异步方法获取数据
      this.seriesDate()
      //设置定时器每十秒自动刷新数据
      this.datatimer=setInterval(this.seriesDate,10000)
    },
    beforeDestroy() {
      clearInterval(this.flashtimer)
    }
 }
</script>

<style scoped>
</style>
