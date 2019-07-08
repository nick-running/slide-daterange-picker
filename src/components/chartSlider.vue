<template>
    <div class="chart-slider">
        <slider @on-date-range-change="onDateRangeChange"
                :total-date-range="totalDateRange"
                :format="format"
                :date-range="dateRange">
            <lineChart></lineChart>
        </slider>
    </div>
</template>

<script>
    import slider from './slider'
  import lineChart from './lineChart.vue'
  export default {
    name: "chart-slider",
    components: {
      slider, lineChart
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
      return {}
    },
    methods: {
      onDateRangeChange(dateRange){
        this.$emit('on-date-range-change', dateRange);
      }
    },
    mounted() {
      
    }
  }
</script>

<style scoped>

</style>