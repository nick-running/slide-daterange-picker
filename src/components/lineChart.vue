<template>
    <div class="line-cart">
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
    name: "line-cart",
    props: {
      data: {
        type: Array,
        default: ()=>[]
      }
    },
    data() {
      return {
        chartData: this.data
      }
    },
    methods: {
      initChart(){
        chart = echarts.init(this.$refs.chart)
      },
      renderChart() {
        console.log('renderChart...')
        // let data = [
        //   {
        //     "name": "业务a",
        //     "id": "a4eaae67-759a-47a9-8414-236a9fcd7d46",
        //     "data": [
        //       {
        //         "ts": "2019-04-15 00:00:23",
        //         "num": "56"
        //       },
        //       {
        //         "ts": "2019-04-15 00:10:23",
        //         "num": "112"
        //       },
        //       {
        //         "ts": "2019-04-15 00:23:23",
        //         "num": "33"
        //       }
        //     ]
        //   }
        // ]
        let seriesData = []
        this.chartData.forEach(n=>{
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
          color: ['#e8e8e8', '#724bbe', '#e4873c', '#2cbdbf', '#d64952'],
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
      },
      resize(){
        chart.resize()
      },
    },
    watch: {
      data(d){
        this.chartData = d
        this.renderChart()
      }
    },
    mounted() {
      this.initChart()
      this.renderChart()
      window.addEventListener('resize', this.resize)
    },
    beforeDestroy(){
      window.removeEventListener('resize', this.resize)
    }
  }
</script>

<style scoped>
    .line-cart{
        display: flex;
        flex: 1;
    }
    .line-cart>.chart{
        flex: 1
    }
</style>