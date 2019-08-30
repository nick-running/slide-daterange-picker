<template>
	<div class="slider3">
		<input v-model="startValue" type="text">
		<input v-model="endValue" type="text">
		<div>
			{{dateRange}}
		</div>
	</div>
</template>

<script>
  export default {
    name: "slider3",
    model: {
      prop: 'dateRange',
      event: 'change'
    },
    props: {
      format: {
        type: String
      },
      totalDateRange: {
        type: String,
      },
      dateRange: {
        type: String
      }
    },
    data() {
      return {
        startValue: this.dateRange.split(',')[0],
        endValue: this.dateRange.split(',')[1],
      }
    },
    methods: {
		updateModel(){
          // this.dateRange[0] = this.startValue
          // this.dateRange[1] = this.endValue
          this.$emit('change', this.startValue+','+this.endValue)
		}
    },
    watch: {
      startValue(v){
        this.updateModel()
      },
      endValue(v){
        this.updateModel()
      },
      dateRange(d){
		this.startValue= d.split(',')[0]
		this.endValue= d.split(',')[1]
      }
    },
    mounted() {
      
    }
  }
</script>

<style scoped lang="scss">
	.slider3{
		height: 30px;
	}
</style>