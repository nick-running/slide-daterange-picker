<template>
  <div id="app">
    <!--<div id="nav">-->
      <!--<router-link to="/">Home</router-link> |-->
      <!--<router-link to="/about">About</router-link>-->
    <!--</div>-->
    <!--<router-view/>-->
        <h1>slide-daterange-picker DEMO</h1>
      <h4 style="text-align: center">{{dateRange}}</h4>
      <slider @on-date-range-change="dateRange=$event"
              :total-date-range="['2019-6-4', '2019-6-5']"
              format="YYYY-MM-DD HH:mm:ss"
              :date-range="['2019-6-4 12:52:00', '2019-6-4 16:43:45']">
          <div>big data</div>
      </slider>
  </div>
</template>
<script>
  import slider from './components/slider'
  export default {
    name: 'home',
    components: {
      slider
    },
    data(){
      return {
        dateRange: null
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
