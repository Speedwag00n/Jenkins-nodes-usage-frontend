<template>
  <div class="chart-container">
    <div class="chart-menu">
      <div class="menu-item" v-for="(item, index) in items" v-bind:key="index">
        <label class="menu-item-label">{{item.name}}</label>
        <input type="radio" :value="item.name" v-model="currentNode" v-on:change="onNodeChange">
      </div>
    </div>
    <GChart
      v-bind:class="{empty: isEmpty}"
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
  props: ['items', 'chartData'],
  components: {
    GChart
  },
  data: () => ({
    currentNode: '',
    chartOptions: {
      chart: {
        title: 'Node usage'
      }
    }
  }),
  methods: {
    onNodeChange: function(){
      this.$emit('nodechange', this.currentNode);
    }
  },
  computed: {
    isEmpty: function() {
      return this.chartData.length == 0;
    }
  }
}
</script>

<style scoped>
.empty {
  display: none;
}
</style>