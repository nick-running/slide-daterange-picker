<template>
	<div class="slider3">
		<input v-model="mValue" type="hidden">
		<div>
			{{mValue}}
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
        mValue: this.dateRange,
      }
    },
    methods: {
		updateModel(){
          this.$emit('change', this.mValue)
		}
    },
    watch: {
      mValue(){
        this.updateModel()
      },
      dateRange(d){
		this.mValue= d
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