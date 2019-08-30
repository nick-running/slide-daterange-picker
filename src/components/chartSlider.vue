<template>
    <div class="chart-slider">
        <slider @on-date-range-change="onDateRangeChange"
                :total-date-range="totalDateRange"
                :format="format"
                v-model="mDateRange">
            <lineChart :data="chartData"></lineChart>
        </slider>
    </div>
</template>

<script>
    import slider from './slider5'
  import lineChart from './lineChart.vue'
  export default {
    name: "chart-slider",
    components: {
      slider, lineChart
    },
    model: {
      prop: 'dateRange',
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
        type: String
      },
      dateRange: {
        type: String
      },
      chartData: {
        type: Array,
        default: ()=>[]
      }
    },
    data() {
      return {
        mDateRange: this.dateRange
      }
    },
    methods: {
      onDateRangeChange(dateRange){
        // console.log('dateRange is: ', JSON.stringify(dateRange))
        this.$emit('on-date-range-change', dateRange);
        this.$emit('change', dateRange)
      }
    },
    watch: {
      dateRange(d){
        this.mDateRange = d
      }
    },
    mounted() {
      
    }
  }
</script>

<style scoped>

</style>