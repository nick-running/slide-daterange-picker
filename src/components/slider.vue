<template>
    <div class="slider" :style="styles">
        <div ref="sliderBox" class="slider-box">
            <div @dragstart="handleRangeDragStart"
                 @drag="handleRangeDrag"
                 @dragover="handleRangeDragOver"
                 @dragend="handleRangeDragEnd"
                 draggable="true" :style="sliderMaskStyles" class="slider-range">
                <div class="slider-bar slider-bar1"></div>
                <div class="slider-bar slider-bar2"></div>
            </div>
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
          offsetLeft: 0,
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
        },
        drag: {
          offsetX: 0,
          startOffsetX: 0,
          draggingOffsetX: 0,
          clientX: 0,
        },
        sliderMaskStyles: {
          left: 0,
          width: 0,
          height: 0
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
        this.box.offsetLeft = this.$refs.sliderBox.offsetLeft
      },
      resize(){
        this.getDomAttr()
      },
      handleRangeDragStart(ev){
        // ev.effectAllowed = 'move'
        this.drag.startOffsetX = ev.layerX

        console.log('开始拖动 is: ', ev)
        // console.log('开始拖动: ', JSON.stringify(this.drag.startOffsetX))
        // let y = ev.offsetY
        // console.log('start.')
        // console.log('x is: ', JSON.stringify(x))
        // console.log('y is: ', JSON.stringify(y))
        // console.log('ev is: ', ev)
      },
      handleRangeDrag(ev){
        ev.preventDefault();
        console.log('拖动: ', ev)
        this.drag.draggingOffsetX = ev.layerX
        this.drag.clientX = ev.clientX
        // console.log('this.drag.draggingOffsetX is: ', JSON.stringify(this.drag.draggingOffsetX))
        // console.log('this.drag.draggingOffsetX is: ', JSON.stringify(this.drag.draggingOffsetX))
        console.log('this.drag.draggingOffsetX-this.drag.startOffsetX is: ', JSON.stringify(this.drag.draggingOffsetX))
        // console.log('this.sliderMaskStyles is: ', JSON.stringify(this.sliderMaskStyles))
        // let y = ev.offsetY
        // console.log('x is: ', JSON.stringify(x))
        // console.log('y is: ', JSON.stringify(y))
        // console.log('ev is: ', ev)

        let left = (this.drag.clientX-this.box.offsetLeft-this.drag.startOffsetX)
        let left2 = left+parseFloat(this.sliderMaskStyles.width)
        this.sliderMaskStyles.left = left+'px'
        let startDateTime = (left*this.scale)+new Date(this.myTotalDateRange.startDate).getTime()
        let endDateTime = (left2*this.scale)+new Date(this.myTotalDateRange.startDate).getTime()
        console.log('startDateTime is: ', JSON.stringify(Math.ceil(startDateTime)))
        console.log('endDateTime is: ', JSON.stringify(Math.ceil(endDateTime)))
      },
      handleRangeDragOver(ev) {
      },
      handleRangeDragEnd(){

        // this.drag.startOffsetX = 0
        // this.drag.draggingOffsetX = 0
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
      // sliderMaskStyles(){
      //   let styles = {}
      //   if (this.box.height&&this.slider1X&&this.slider2X) {
      //     styles.height = this.box.height+'px'
      //     // styles.left = this.slider1X+'px'
      //     // styles.left = (this.slider1X+this.drag.draggingOffsetX-this.drag.startOffsetX)+'px'
      //     styles.left = (this.slider1X+this.drag.clientX-this.drag.draggingOffsetX-this.drag.startOffsetX)+'px'
      //     styles.width = (this.slider2X-this.slider1X)+'px'
      //   }
      //   return styles
      // }
    },
    watch: {
      'box.height'(h) {
        this.sliderMaskStyles.height = h+'px'
      },
      slider1X(x) {
        this.sliderMaskStyles.left = x+'px'
      },
      slider2X(x) {
        this.sliderMaskStyles.width = (x-this.slider1X)+'px'
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
    .slider-range{
        position: absolute;
        background-color: rgba(0, 0, 0, 0.2);
        display: flex;
        justify-content: space-between;
        cursor: ew-resize;
    }
    .slider-range>.slider-bar{
        width: 5px;
        background-color: red;
    }
</style>