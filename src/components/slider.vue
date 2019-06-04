<template>
    <div class="slider" :style="styles">
        <div ref="sliderBox" class="slider-box">
            <div :style="sliderMaskStyles" class="slider-mask"></div>
            <div :style="sliderStyles1" class="slider-bar slider-bar1"></div>
            <div :style="sliderStyles2" class="slider-bar slider-bar2"></div>
        </div>
    </div>
</template>

<script>
  export default {
    name: "slider",
    props: {
      height: {
        default: 30
      },
      totalDateRange: {
        type: Array,
        default(){  // 默认起止为当前时间
          return [new Date(), new Date()]
        }
      },
      dateRange: {
        type: Array,
        default(){  // 默认起止为当前时间
          return [new Date(), new Date()]
        }
      }
    },
    data() {
      return {
        box: {
          width: 0,
          height: 0
        },
        myTotalDateRange: {
          startDate: null,
          endDate: null
        },
        myDateRange: {
          startDate: null,
          endDate: null
        }
      }
    },
    methods: {
      init(){
        this.myTotalDateRange.startDate = this.$moment(this.totalDateRange[0])
        this.myTotalDateRange.endDate = this.$moment(this.totalDateRange[1])
        this.myDateRange.startDate = this.$moment(this.dateRange[0])
        this.myDateRange.endDate = this.$moment(this.dateRange[1])
      },
      getDomAttr(){
        this.box.width = this.$refs.sliderBox.clientWidth
        this.box.height = this.$refs.sliderBox.clientHeight
      },
      resize(){
        this.getDomAttr()
      }
    },
    computed: {
      styles() {
        let styles = {}
        if(this.height){
          styles.height = parseInt(this.height)+'px'
        }
        return styles;
      },
      sliderStyles() {
        let styles = {}
        if (this.box.height) {
          styles.height = this.box.height+'px'
        }
        return styles
      },
      sliderStyles1(){
        let styles = Object.assign({}, this.sliderStyles)
        if (this.slider1X) {
          styles.left = this.slider1X+'px'
        }
        return styles
      },
      sliderStyles2(){
        let styles = Object.assign({}, this.sliderStyles)
        if (this.slider2X) {
          styles.left = this.slider2X+'px'
        }
        return styles
      },
      timeLength(){
        return this.myTotalDateRange.endDate-this.myTotalDateRange.startDate
      },
      scale(){
        return this.timeLength/this.box.width
      },
      slider1X(){
        let startDate = this.myDateRange.startDate-this.myTotalDateRange.startDate
        return startDate/this.scale
      },
      slider2X(){
        let endDate = this.myDateRange.endDate-this.myTotalDateRange.startDate
        return endDate/this.scale
      },
      sliderMaskStyles(){
        let styles = {}
        if (this.box.height&&this.slider1X&&this.slider2X) {
          styles.height = this.box.height+'px'
          styles.left = this.slider1X+'px'
          styles.width = (this.slider2X-this.slider1X)+'px'
        }
        return styles
      }
    },
    created(){
      this.init()
      // console.log('this.myTotalDateRange is: ', JSON.stringify(this.myTotalDateRange))
    },
    mounted() {
      this.getDomAttr()
      window.addEventListener('resize', this.resize, false)
    },
    beforeDestroy(){
      window.removeEventListener('resize', this.resize, false)
    }
  }
</script>

<style scoped>
    .slider{
        width: 100%;
        background-color: #fff;
        border: 1px solid #ddd;
        display: flex;
    }
    .slider-box{
        flex: 1;
        position: relative;
        display: flex;
    }
    .slider-box>.slider-bar{
        position: absolute;
        width: 5px;
        background-color: red;
    }
    .slider-mask{
        position: absolute;
        background-color: rgba(0, 0, 0, 0.2);
    }
</style>