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
  components: {
    GChart
  },
  data () {
    return {
      chartData: [
        ['Дата', 'Работала'],
        ['2019-12-15', 8],
        ['2019-12-14', 7],
        ['2019-12-13', 6],
        ['2019-12-12', 4],
        ['2019-12-11', 9],
        ['2019-12-10', 12],
        ['2019-12-09', 1]
      ],
      chartOptions: {
        chart: {
          title: 'Загруженность ноды'
        }
      }
    }
  }
}
</script>

<style scoped>

</style>