<template>
    <div class="chartSlider">
        <!--<div ref="chart" style="width: 800px; height:400px"></div>-->
        <div class="chart" ref="chart"></div>
    </div>
</template>

<script>
  let echarts = require('echarts');
  require('echarts/lib/chart/bar');
  require('echarts/lib/component/tooltip');
  require('echarts/lib/component/title');
  let chart
  export default {
    name: "chartSlider",
    data() {
      return {}
    },
    methods: {
      initChart(){
        chart = echarts.init(this.$refs.chart)
      },
      renderChart() {
        let data = [
          {
            "name": "业务a",
            "id": "a4eaae67-759a-47a9-8414-236a9fcd7d46",
            "data": [
              {
                "ts": "2019-04-15 00:00:23",
                "num": "122"
              },
              {
                "ts": "2019-04-15 00:10:23",
                "num": "112"
              },
              {
                "ts": "2019-04-15 00:23:23",
                "num": "0"
              }
            ]
          }
        ]
        let seriesData = []
        data.forEach(n=>{
          let itemList = []
          n.data.forEach(d=>{
            itemList.push([d.ts, d.num])
          })
          seriesData.push({
            sampling: 'average',
            name: n.name,
            type: 'line',
            symbol: 'circle',
            smooth: true,
            areaStyle: {},
            data: itemList
          })
        })
        let option = {
          color: ['#299def', '#724bbe', '#e4873c', '#2cbdbf', '#d64952'],
          grid: {
            top: '0',
            left: '0',
            right: '0',
            bottom: '0',
            containLabel: false
          },
          xAxis: {
            type: 'time',
            show: false,
            boundaryGap: false,
            splitLine: {
              show: false
            }
          },
          yAxis: {
            type: 'value',
            show: false,
            splitLine: {
              show: false
            }
          },
          series: seriesData
        };
        chart.setOption(option)
      }
    },
    mounted() {
      this.initChart()
      this.renderChart()
    }
  }
</script>

<style scoped>
    .chartSlider{
        display: flex;
        flex: 1;
    }
    .chartSlider>.chart{
        flex: 1
    }
</style>