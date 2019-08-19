<template>
  <div id="app">
    <!--<div id="nav">-->
      <!--<router-link to="/">Home</router-link> |-->
      <!--<router-link to="/about">About</router-link>-->
    <!--</div>-->
    <!--<router-view/>-->
        <h1>slide-daterange-picker demo</h1>
      <h4 style="text-align: center">{{dateRange}}</h4>
      <slider @on-date-range-change="dateRange=$event"
              :total-date-range="['2019-6-4 00:00:00', '2019-6-4 01:00:01']"
              format="YYYY-MM-DD HH:mm:ss"
              :date-range="['2019-6-4 00:12:00', '2019-6-4 00:23:00']">
          <!--<div>big data</div>-->
      </slider>
      <h4 style="text-align: center">{{dateRange2}}</h4>
      <!--<slider @on-date-range-change="dateRange2=$event"-->
              <!--:total-date-range="['2019-6-4 00:00:00', '2019-6-4 23:00:01']"-->
              <!--format="YYYY-MM-DD HH:mm:ss"-->
              <!--:date-range="['2019-6-4 00:12:00', '2019-6-4 00:23:00']">-->
              <!--<chartSlider></chartSlider>-->
      <!--</slider>-->
      <div style="display: flex">
          <div style="width: 150px;flex: 0 0 150px;">
              left
          </div>
          <div style="flex: 1">
              <chartSlider @on-date-range-change="dateRange2=$event"
                           :total-date-range="['2019-6-4 00:00:00', '2019-6-4 23:00:01']"
                           format="YYYY-MM-DD HH:mm:ss"
                           :date-range="['2019-6-4 00:12:00', '2019-6-4 00:23:00']"/>
          </div>
      </div>


      <!--<div style="height: 500px;position: relative;">-->
          <!--<div style="position: absolute;width: 100%;height:100%;display: flex;">-->
              <!--<chartSlider></chartSlider>-->
          <!--</div>-->
      <!--</div>-->
  </div>
</template>
<script>
  import slider from './components/slider'
  import chartSlider from './components/chartSlider'
  export default {
    name: 'home',
    components: {
      slider, chartSlider
    },
    data(){
      return {
        dateRange: null,
        dateRange2: null,
      }
    },
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
