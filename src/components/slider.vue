<template>
    <div class="slider">
        <div class="slider-info">{{sliderInfo.text1}}</div>
        <div class="slider-wrapper" :style="styles">
            <div ref="sliderBox" class="slider-box">
                <div class="extra-content">
                    <slot></slot>
                </div>
                <div @mousedown="handleRangeDragStart"
                     :style="sliderMaskStyles" class="slider-range">
                    <div @mousedown="handleSliderBarDragStart"
                         class="slider-bar slider-bar1"></div>

                    <div @mousedown="handleSliderBar2DragStart"
                         class="slider-bar slider-bar2"></div>
                </div>
            </div>
        </div>
        <div class="slider-info slider-info2">{{sliderInfo.text2}}</div>
    </div>
</template>

<script>
  export default {
    name: "slider",
    model: {
      prop: 'myDateRange',
      event: 'change'
    },
    props: {
      height: {
        type: Number,
        default: 30
      },
      format: {
        type: String
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
          height: 0,
          maxLeft: 0 // 容器最大长度
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
          active: false,
          offsetX: 0,
          startOffsetX: 0,
          draggingOffsetX: 0,
          clientX: 0,
        },
        dragSlider1: {
          active: false,
          offsetX: 0,
          startOffsetX: 0,
          draggingOffsetX: 0,
          clientX: 0,
        },
        dragSlider2: {
          active: false,
          offsetX: 0,
          startOffsetX: 0,
          draggingOffsetX: 0,
          clientX: 0,
        },
        sliderMaskStyles: {
          left: 0,
          width: 0,
          height: 0
        },
        sliderInfo: {
          text1: '',
          text2: ''
        }
      }
    },
    methods: {
      init(){
        this.myTotalDateRange.startDate = this.$moment(this.totalDateRange[0])
        this.myTotalDateRange.endDate = this.$moment(this.totalDateRange[1])

        let dateRange = this.dateRangeFormat(this.myTotalDateRange)
        this.sliderInfo.text1 = dateRange[0]
        this.sliderInfo.text2 = dateRange[1]

        this.myDateRange.startDate = this.$moment(this.dateRange[0])
        this.myDateRange.endDate = this.$moment(this.dateRange[1])
      },
      getDomAttr(){
        this.box.width = this.$refs.sliderBox.clientWidth
        this.box.height = this.$refs.sliderBox.clientHeight
        this.box.offsetLeft = this.$refs.sliderBox.getBoundingClientRect().left
        // console.log('this.$refs.sliderBox is: ', this.$refs.sliderBox.getBoundingClientRect().left)
        this.box.maxLeft = this.$refs.sliderBox.clientWidth
        // console.log('this.$refs.sliderBox.offsetLeft is: ', JSON.stringify(this.$refs.sliderBox.offsetLeft))
        // console.log('this.$refs.sliderBox.clientLeft is: ', JSON.stringify(this.$refs.sliderBox.clientLeft))
        // console.log('this.$refs.sliderBox.clientWidth is: ', JSON.stringify(this.$refs.sliderBox.clientWidth))
      },
      dateRangeFormat(rangeDate) {
        let dateRange = []
        if (this.format) {
          const _this = this
          let formatDate = (date)=>_this.$moment(date).format(_this.format)
          dateRange[0] = formatDate(rangeDate.startDate)
          dateRange[1] = formatDate(rangeDate.endDate)
        }else{
          dateRange = this.myDateRange
        }
        return dateRange
      },
      resize(){
        // console.log('resize.')
        this.getDomAttr()
      },
      move(ev) {
        ev.stopPropagation();
        if(!this.drag.active&&!this.dragSlider1.active&&!this.dragSlider2.active) return
        if (this.drag.active) {
            this.handleRangeDrag(ev)
        }else if (this.dragSlider1.active) {
            this.handleSliderBarDrag(ev)
        }else if (this.dragSlider2.active) {
          this.handleSliderBar2Drag(ev)
        }
        this.setTime()
        // console.log('ev is: ', ev)
      },
      up() {
        if(!this.drag.active&&!this.dragSlider1.active&&!this.dragSlider2.active) return
        if (this.drag.active) {
            this.drag.active = false
        }else if (this.dragSlider1.active) {
            this.dragSlider1.active = false
        }else if (this.dragSlider2.active) {
            this.dragSlider2.active = false
        }
        this.setTime()
      },
      setTime(){
        // debugger
        let left = parseFloat(this.sliderMaskStyles.left)
        let left2 = left+parseFloat(this.sliderMaskStyles.width)
        let startDateTime = (left*this.scale)+new Date(this.myTotalDateRange.startDate).getTime()
        let endDateTime = (left2*this.scale)+new Date(this.myTotalDateRange.startDate).getTime()
        console.log('setTime...')
        // console.log('startDateTime is: ', JSON.stringify(Math.ceil(startDateTime)))
        // console.log('endDateTime is: ', JSON.stringify(Math.ceil(endDateTime)))
        this.myDateRange.startDate = this.$moment(Math.ceil(startDateTime))
        this.myDateRange.endDate = this.$moment(Math.ceil(endDateTime))
        let dateRange = []
        if (this.format) {
          const _this = this
          let formatDate = (date)=>_this.$moment(date).format(_this.format)
          dateRange[0] = formatDate(this.myDateRange.startDate)
          dateRange[1] = formatDate(this.myDateRange.endDate)
        }else{
          dateRange = this.myDateRange
        }
        this.$emit('on-date-range-change', dateRange);
      },
      handleRangeDragStart(ev){
        this.drag.active = true
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
        if(!this.drag.active) return
        console.log('拖动: ', ev)
        this.drag.draggingOffsetX = ev.layerX
        this.drag.clientX = ev.clientX
        // console.log('this.drag.draggingOffsetX is: ', JSON.stringify(this.drag.draggingOffsetX))
        // console.log('this.drag.draggingOffsetX is: ', JSON.stringify(this.drag.draggingOffsetX))
        // console.log('this.drag.draggingOffsetX-this.drag.startOffsetX is: ', JSON.stringify(this.drag.draggingOffsetX))
        // console.log('this.sliderMaskStyles is: ', JSON.stringify(this.sliderMaskStyles))
        // let y = ev.offsetY
        // console.log('x is: ', JSON.stringify(x))
        // console.log('y is: ', JSON.stringify(y))
        // console.log('ev is: ', ev)

        let left = (this.drag.clientX-this.box.offsetLeft-this.drag.startOffsetX)
        // let left2 = left+parseFloat(this.sliderMaskStyles.width)
        // console.log('left is: ', JSON.stringify(left))
        // console.log('this.box.maxLeft-this.sliderMaskStyles.width is: ', JSON.stringify(this.box.maxLeft-this.sliderMaskStyles.width))
        if (left >= 0&&left<=this.box.maxLeft-parseFloat(this.sliderMaskStyles.width)) {
            this.sliderMaskStyles.left = left+'px';
        }else if (left < 0) {
          this.sliderMaskStyles.left = '0px'
        }else if (left>this.box.maxLeft-parseFloat(this.sliderMaskStyles.width)) {
          this.sliderMaskStyles.left = this.box.maxLeft-parseFloat(this.sliderMaskStyles.width)
        }
        // let startDateTime = (left*this.scale)+new Date(this.myTotalDateRange.startDate).getTime()
        // let endDateTime = (left2*this.scale)+new Date(this.myTotalDateRange.startDate).getTime()
        // console.log('startDateTime is: ', JSON.stringify(Math.ceil(startDateTime)))
        // console.log('endDateTime is: ', JSON.stringify(Math.ceil(endDateTime)))
      },
      // handleRangeDragEnd(){
      //   this.drag.active = false
      //   // this.drag.startOffsetX = 0
      //   // this.drag.draggingOffsetX = 0
      // },
      handleSliderBarDragStart(ev) {
        ev.stopPropagation()
        // ev.effectAllowed = 'move'
        this.dragSlider1.active = true
        this.dragSlider1.startOffsetX = ev.layerX
      },
      handleSliderBarDrag(ev) { // 第一块滑块
        if(!this.dragSlider1.active) return
        // console.log('handleSliderBarDrag ev is: ', ev)
        this.dragSlider1.clientX = ev.clientX
        // console.log('this.dragSlider1.startOffsetX is: ', JSON.stringify(this.dragSlider1.startOffsetX))

        let lastLeft = this.sliderMaskStyles.left
        // console.log('this.dragSlider1.startOffsetX is: ', JSON.stringify(this.dragSlider1.startOffsetX))
        let left = this.dragSlider1.clientX-this.box.offsetLeft-this.dragSlider1.startOffsetX
        let left2 = parseFloat(lastLeft)+parseFloat(this.sliderMaskStyles.width)
        // console.log('left-parseFloat(lastLeft) is: ', JSON.stringify(left-parseFloat(lastLeft)))
        if (left >= 0&&left<parseFloat(this.sliderMaskStyles.left)+parseFloat(this.sliderMaskStyles.width)) {
          this.sliderMaskStyles.left = left+'px';
          // this.sliderMaskStyles.width = (parseFloat(this.sliderMaskStyles.width)+this.dragSlider1.startOffsetX)+'px'
          this.sliderMaskStyles.width = (parseFloat(left2)-left)+'px'
        }else if (left < 0) {
          this.sliderMaskStyles.left = '0px'
          this.sliderMaskStyles.width = (parseFloat(left2)-0)+'px'
        }

      },
      // handleSliderBarDragEnd(){
      //   this.dragSlider1.active = false
      // },
      handleSliderBar2DragStart(ev) {
        ev.stopPropagation()
        // ev.effectAllowed = 'move'
        this.dragSlider2.active = true
        this.dragSlider2.startOffsetX = ev.layerX
      },
      handleSliderBar2Drag(ev) { // 第一块滑块
        if (!this.dragSlider2.active) return
        // console.log('handleSliderBarDrag ev is: ', ev)
        this.dragSlider2.clientX = ev.clientX
        // console.log('this.dragSlider2.startOffsetX is: ', JSON.stringify(this.dragSlider2.startOffsetX))

        let lastLeft = this.sliderMaskStyles.left
        // console.log('this.dragSlider2.startOffsetX is: ', JSON.stringify(this.dragSlider2.startOffsetX))
        let left = this.dragSlider2.clientX-this.box.offsetLeft-this.dragSlider2.startOffsetX
        // let left2 = parseFloat(lastLeft)+parseFloat(this.sliderMaskStyles.width)
        // console.log('left-parseFloat(lastLeft) is: ', JSON.stringify(left-parseFloat(lastLeft)))
        // this.sliderMaskStyles.left = left+'px'
        // this.sliderMaskStyles.width = (parseFloat(this.sliderMaskStyles.width)+this.dragSlider2.startOffsetX)+'px'
        let width = this.dragSlider2.clientX-this.box.offsetLeft+this.dragSlider2.startOffsetX-parseFloat(this.sliderMaskStyles.left)

        if (parseFloat(this.sliderMaskStyles.left) + width < this.box.maxLeft) {
          this.sliderMaskStyles.width = width+'px'
        }else if(width>0){
          this.sliderMaskStyles.width = this.box.maxLeft-parseFloat(this.sliderMaskStyles.left)+'px'
        }
        // if (left >= 0&&left<=this.box.maxLeft-parseFloat(this.sliderMaskStyles.width)) {
        //   this.sliderMaskStyles.left = left+'px';
        //   // this.sliderMaskStyles.width = (parseFloat(this.sliderMaskStyles.width)+this.dragSlider1.startOffsetX)+'px'
        //   this.sliderMaskStyles.width = (parseFloat(left2)-left)+'px'
        // }else if (left < 0) {
        //   this.sliderMaskStyles.left = '0px'
        // }
      },
      // handleSliderBar2DragEnd() {
      //   this.dragSlider2.active = false
      // }
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
      window.addEventListener('resize', this.resize)
      document.addEventListener('mousemove', this.move)
      document.addEventListener('mouseup', this.up)
      // console.log('this.$moment(1559615513925) is: ', JSON.stringify(this.$moment(1559615513925)))
    },
    beforeDestroy(){
      window.removeEventListener('resize', this.resize)
      document.removeEventListener('mouseup', this.up)
    }
  }
</script>

<style scoped>
    .slider{
        display: flex;
    }
    .slider>.slider-info{
        width: 160px;
        display: flex;
        align-items: center;
    }
    .slider>.slider-info2{
        justify-content: flex-end;
    }
    .slider>.slider-wrapper{
        flex:1;
    }
    .slider-wrapper{
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
        background-color: rgba(0, 0, 0, 0.15);
        display: flex;
        /*justify-content: space-between;*/
        /*cursor: ew-resize;*/
    }
    .slider-range>.slider-bar{
        position: absolute;
        width: 5px;
        height: 100%;
        background-color: #2d8cf0;
        cursor: ew-resize;
    }
    .slider-range>.slider-bar1{
        left:0;
    }
    .slider-range>.slider-bar2{
        right:0;
    }
    .extra-content{
        position: absolute;
        width: 100%;
        height: 100%;
        display: flex;
        /*flex-direction: column;*/
        /*justify-content: center;*/
        /*align-items: center;*/
    }
    div {
        -moz-user-select:none;
        -webkit-user-select:none;
        user-select:none;
    }
</style>