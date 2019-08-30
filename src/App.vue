<template>
  <div id="app">
	  外部
	  <!--<div style="border: 1px solid darkgrey;padding: 5px">-->
		  <!--<input @input="handleSlider3Input" v-model="slider3Start" type="text">-->
		  <!--<input @input="handleSlider3Input" v-model="slider3End" type="text">-->
		  <!--<div>-->
			  <!--slider3: {{slider3}}-->
		  <!--</div>-->
	  <!--</div>-->
	  <!--组件slider3-->
	  <h4>dateRange5</h4>
	  <input v-model="dateRange5" type="text" style="width:300px">
	  <slider5 v-model="dateRange5" format="YYYY-MM-DD HH:mm:ss"
	           :totalDateRange="totalDateRange5"></slider5>
	  <div>
		  dateRange5: {{dateRange5}}
	  </div>
	  <!--<slider3 v-model="slider3"-->
	           <!--:totalDateRange="slider3"></slider3>-->
	  <!--SingleDate:-->
	  <!--<SingleDate v-model="range"></SingleDate>-->
    <!--&lt;!&ndash;<div id="nav">&ndash;&gt;-->
      <!--&lt;!&ndash;<router-link to="/">Home</router-link> |&ndash;&gt;-->
      <!--&lt;!&ndash;<router-link to="/about">About</router-link>&ndash;&gt;-->
    <!--&lt;!&ndash;</div>&ndash;&gt;-->
    <!--&lt;!&ndash;<router-view/>&ndash;&gt;-->
        <!--<h1>slide-daterange-picker demo</h1>-->
      <!--<h4 style="text-align: center">slider2: {{dateRange}}</h4>-->
      <!--<slider2 @on-date-range-change="dateRange=$event"-->
              <!--:total-date-range="['2019-6-4 00:00:00', '2019-6-4 01:00:01']"-->
              <!--format="YYYY-MM-DD HH:mm:ss"-->
              <!--v-model="dateRange">-->
          <!--&lt;!&ndash;<div>big data</div>&ndash;&gt;-->
      <!--</slider2>-->
      <!--<h4 style="text-align: center">{{dateRange2}}</h4>-->
      <!--&lt;!&ndash;<slider @on-date-range-change="dateRange2=$event"&ndash;&gt;-->
              <!--&lt;!&ndash;:total-date-range="['2019-6-4 00:00:00', '2019-6-4 23:00:01']"&ndash;&gt;-->
              <!--&lt;!&ndash;format="YYYY-MM-DD HH:mm:ss"&ndash;&gt;-->
              <!--&lt;!&ndash;:date-range="['2019-6-4 00:12:00', '2019-6-4 00:23:00']">&ndash;&gt;-->
              <!--&lt;!&ndash;<chartSlider></chartSlider>&ndash;&gt;-->
      <!--&lt;!&ndash;</slider>&ndash;&gt;-->
      <!--<div style="display: flex">-->
          <!--<div style="width: 150px;flex: 0 0 150px;">-->
              <!--left-->
          <!--</div>-->
          <!--<div style="flex: 1">-->
	          <!--&lt;!&ndash;<chartSlider @on-date-range-change="dateRange2=$event"&ndash;&gt;-->
	                       <!--&lt;!&ndash;:total-date-range="['2019-6-4 00:00:00', '2019-6-4 23:00:01']"&ndash;&gt;-->
	                       <!--&lt;!&ndash;format="YYYY-MM-DD HH:mm:ss"&ndash;&gt;-->
	                       <!--&lt;!&ndash;:date-range="['2019-6-4 00:12:00', '2019-6-4 00:23:00']"/>&ndash;&gt;-->
	          <chartSlider
	                       :total-date-range="totalDateRange5"
	                       format="YYYY-MM-DD HH:mm:ss"
	                       v-model="dateRange5" :chartData="chartData"/>
          <!--</div>-->
      <!--</div>-->
	  <!--开始：<input type="text" v-model="editor.date[0]">-->
	  <!--结束：<input type="text" v-model="editor.date[1]">-->


      <!--<div style="height: 500px;position: relative;">-->
          <!--<div style="position: absolute;width: 100%;height:100%;display: flex;">-->
              <!--<chartSlider></chartSlider>-->
          <!--</div>-->
      <!--</div>-->
  </div>
</template>
<script>
  import slider from './components/slider'
  import slider2 from './components/slider2'
  import slider3 from './components/slider3'
  import slider4 from './components/slider4'
  import slider5 from './components/slider5'
  import SingleDate from './components/SingleDate'
  import chartSlider from './components/chartSlider'
  export default {
    name: 'home',
    components: {
      slider, slider2, slider3, slider4, slider5, chartSlider, SingleDate
    },
    data(){
      return {
        slider3: '1,1',
        slider3Start: '0',
        slider3End: '0',
        range: [28, 3],
        dateRange: ['2019-6-4 00:12:00', '2019-6-4 00:23:00'],
        dateRange2: ['2019-6-4 00:12:00', '2019-6-4 00:23:00'],
        editor: {
          date: ['2019-06-04 00:00:00', '2019-6-4 00:23:00']
        },
        dateRange5: '2019-6-4 06:00:00,2019-6-4 15:07:34',
        totalDateRange5: '2019-6-4 00:00:00,2019-6-4 23:59:59',
        chartData: [
          {
            "name": "业务a",
            "id": "a4eaae67-759a-47a9-8414-236a9fcd7d46",
            "data": [
              {
                "ts": "2019-04-15 00:00:23",
                "num": "56"
              },
              {
                "ts": "2019-04-15 00:10:23",
                "num": "112"
              },
              {
                "ts": "2019-04-15 00:23:23",
                "num": "33"
              }
            ]
          }
        ]
      }
    },
    methods: {
      handleSlider3Input(){
        this.slider3 = this.slider3Start+','+this.slider3End
      }
    },
    watch: {
      slider3: {
        handler(d){
          // console.log('app d is: ', JSON.stringify(d))
          this.slider3Start = d.split(',')[0]
          this.slider3End = d.split(',')[1]
        }
      },
      'editor.date'(d){
        // console.log('d is: ', JSON.stringify(d))
      }
    },
    created(){
      const _this = this
      this.slider3Start = this.slider3.split(',')[0]
      this.slider3End = this.slider3.split(',')[1]
      setTimeout(function () {
        _this.chartData = [
          {
            "name": "业务a",
            "id": "a4eaae67-759a-47a9-8414-236a9fcd7d46",
            "data": [
              {
                "ts": "2019-04-15 00:00:23",
                "num": "256"
              },
              {
                "ts": "2019-04-15 00:10:23",
                "num": "12"
              },
              {
                "ts": "2019-04-15 00:23:23",
                "num": "330"
              }
            ]
          }
        ]
      }, 1000)
      // setInterval(function () {
      //   _this.slider3[0] = Math.random()+''
      // }, 800)
    }
  }
</script>
<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
