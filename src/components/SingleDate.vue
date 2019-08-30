<template>
	<div class="slider">
		<div class="slider-info">{{sliderInfo.text1}}</div>
		<div class="slider-wrapper" :style="styles">
			<div ref="sliderBox" class="slider-box">
				<div class="extra-content">
					<slot></slot>
				</div>
				<div class="slider-content">
					<input v-model="slider1" @click="gogo" class="ne-range" type="range" min="0" max="100" step="0.1"/>
					<input v-model="slider2" @click="gogo" class="ne-range slider-bar2" type="range" min="0" max="100" step="0.1"/>
				</div>
			</div>
		</div>
		<div class="slider-info slider-info2">{{sliderInfo.text2}}</div>
	</div>
</template>

<script>
  export default {
    name: "SingleDate",
    props: {
      value: {
        type: Array
      },
      height: {
        type: Number,
        default: 30
      },
    },
    data() {
      return {
        slider1: 0,
        slider2: 0,
        range: this.value,
        sliderInfo: {
          text1: '',
          text2: ''
        }
      }
    },
    methods: {
      gogo(ev){
        ev.preventDefault()
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
    },
    watch: {
      slider1(v){
        this.range[0] = v
        this.$emit('input', this.range)
      },
      slider2(v){
        this.range[1] = v
        this.$emit('input', this.range)
      },
      range(d){
        this.slider1 = d[0]
        this.slider2 = d[1]
      }
    },
    mounted() {
      this.slider1 = this.value[0]
      this.slider2 = this.value[1]
    }
  }
</script>

<style scoped>
	input[type=range] {
		-webkit-appearance: none;
		border-radius: 10px; /*这个属性设置使填充进度条时的图形为圆角*/
		position: absolute;
		width: 100%;
		margin: 0;
		background-color: transparent;
	}
	input[type=range]::-webkit-slider-thumb {
		-webkit-appearance: none;
	}
	input[type=range]::-webkit-slider-runnable-track {
		height: 30px;
		/*border: 1px solid #ddd;*/
		/*border-radius: 10px; !*将轨道设为圆角的*!*/
		/*box-shadow: 0 1px 1px #def3f8, inset 0 .125em .125em #0d1112; !*轨道内置阴影效果*!*/
	}

	input[type=range]:focus {
		outline: none;
	}
	input[type=range]::-webkit-slider-thumb {
		-webkit-appearance: none;
		height: 30px;
		width: 5px;
		background: #2d8cf0;
		border-radius: 0; /*外观设置为圆形*/
		/*border: solid 0.125em rgba(205, 224, 230, 0.5); !*设置边框*!*/
		/*box-shadow: 0 .125em .125em #3b4547; !*添加底部阴影*!*/
		cursor: ew-resize;
	}

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
	.extra-content{
		position: absolute;
		width: 100%;
		height: 100%;
		display: flex;
	}
	.slider-content{
		z-index: 1;
	}

	.slider-bar2{
		z-index: 2
	}
</style>