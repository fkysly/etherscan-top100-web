<template>
  <div class="hello">
    <chart :options="option1" auto-resize></chart>
    <chart :options="option2" auto-resize></chart>
  </div>
</template>

<script>
import ECharts from 'vue-echarts/components/ECharts'
import axios from 'axios'
import moment from 'moment'

export default {
  name: 'HelloWorld',
  data: function () {
    return  {
      dates: [],
      holdCounts: [],
      addressCounts: [],
      minholdCount: Number.MAX_VALUE,
      maxholdCount: Number.MIN_VALUE,
      minaddressCount: Number.MAX_VALUE,
      maxaddressCount: Number.MIN_VALUE
    }
  },
  computed: {
    option1: function() {
      return {
        tooltip: {
          trigger: 'axis',
          position: function (pt) {
            return [pt[0], '10%'];
          }
        },
        title: {
          left: 'center',
          text: '',
        },
        toolbox: {
          feature: {
              dataZoom: {
                  yAxisIndex: 'none'
              },
              restore: {},
              saveAsImage: {}
          }
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: this.dates
        },
        yAxis: {
          type: 'value',
          boundaryGap: [0, '100%'],
          min: this.minholdCount,
          maxholdCount: this.maxholdCount
        },
        dataZoom: [{
          type: 'inside',
          start: 0,
          end: 10
        }, {
          start: 0,
          end: 10,
          handleIcon: 'M10.7,11.9v-1.3H9.3v1.3c-4.9,0.3-8.8,4.4-8.8,9.4c0,5,3.9,9.1,8.8,9.4v1.3h1.3v-1.3c4.9-0.3,8.8-4.4,8.8-9.4C19.5,16.3,15.6,12.2,10.7,11.9z M13.3,24.4H6.7V23h6.6V24.4z M13.3,19.6H6.7v-1.4h6.6V19.6z',
          handleSize: '80%',
          handleStyle: {
            color: '#fff',
            shadowBlur: 3,
            shadowColor: 'rgba(0, 0, 0, 0.6)',
            shadowOffsetX: 2,
            shadowOffsetY: 2
          }
        }],
        series: [
          {
            name:'模拟数据',
            type:'line',
            smooth:true,
            symbol: 'none',
            sampling: 'average',
            itemStyle: {
              normal: {
                color: 'rgb(255, 70, 131)'
              }
            },
            areaStyle: {
              normal: {
                color: new ECharts.graphic.LinearGradient(0, 0, 0, 1, [{
                    offset: 0,
                    color: 'rgb(255, 158, 68)'
                }, {
                    offset: 1,
                    color: 'rgb(255, 70, 131)'
                }])
              }
            },
            data: this.holdCounts
          }
        ]
      }
    },
    option2: function() {
      return {
        tooltip: {
          trigger: 'axis',
          position: function (pt) {
            return [pt[0], '10%'];
          }
        },
        title: {
          left: 'center',
          text: '',
        },
        toolbox: {
          feature: {
              dataZoom: {
                  yAxisIndex: 'none'
              },
              restore: {},
              saveAsImage: {}
          }
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: this.dates
        },
        yAxis: {
          type: 'value',
          boundaryGap: [0, '100%'],
          min: this.minaddressCount,
          max: this.maxaddressCount
        },
        dataZoom: [{
          type: 'inside',
          start: 0,
          end: 10
        }, {
          start: 0,
          end: 10,
          handleIcon: 'M10.7,11.9v-1.3H9.3v1.3c-4.9,0.3-8.8,4.4-8.8,9.4c0,5,3.9,9.1,8.8,9.4v1.3h1.3v-1.3c4.9-0.3,8.8-4.4,8.8-9.4C19.5,16.3,15.6,12.2,10.7,11.9z M13.3,24.4H6.7V23h6.6V24.4z M13.3,19.6H6.7v-1.4h6.6V19.6z',
          handleSize: '80%',
          handleStyle: {
            color: '#fff',
            shadowBlur: 3,
            shadowColor: 'rgba(0, 0, 0, 0.6)',
            shadowOffsetX: 2,
            shadowOffsetY: 2
          }
        }],
        series: [
          {
            name:'模拟数据',
            type:'line',
            smooth:true,
            symbol: 'none',
            sampling: 'average',
            itemStyle: {
              normal: {
                color: 'rgb(255, 70, 131)'
              }
            },
            areaStyle: {
              normal: {
                color: new ECharts.graphic.LinearGradient(0, 0, 0, 1, [{
                    offset: 0,
                    color: 'rgb(255, 158, 68)'
                }, {
                    offset: 1,
                    color: 'rgb(255, 70, 131)'
                }])
              }
            },
            data: this.addressCounts
          }
        ]
      }
    }
  },
  created: function() {
    moment.locale('zh-CN')

    axios
    .get('http://203.195.233.78:8080/api/records')
    .then(response => {
      let data = response.data
      data.forEach(item => {
        this.holdCounts.push(item.holdCount)
        this.minholdCount = Math.min(this.minholdCount, item.holdCount)
        this.maxholdCount = Math.max(this.maxholdCount, item.holdCount)

        let date = moment(item.date).format('MM-DD HH:mm')
        this.dates.push(date)

        this.addressCounts.push(item.addressCount)
        this.minaddressCount = Math.min(this.minaddressCount, item.addressCount)
        this.maxaddressCount = Math.max(this.maxaddressCount, item.addressCount)
      })
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
a {
  color: #42b983;
}
.hello, .echarts {
  margin: auto;
  width: 100%;
  height: 70%;
}

</style>
