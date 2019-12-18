<template>
  <div class="chart-container">
    <GChart
      type="ColumnChart"
      :data="chartData"
      :options="chartOptions"
    />
  </div>
</template>

<script>
import { GChart } from 'vue-google-charts'

export default {
  name: 'Chart',
  props: ['chartData'],
  components: {
    GChart
  },
  data: () => ({
    chartOptions: {
      chart: {
        title: 'Node usage'
      }
    }
  })
}
</script>

<style scoped>

</style>