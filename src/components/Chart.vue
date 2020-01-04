<template>
  <div class="chart-container">
    <div class="chart-menu-container">
      <h1 class="main-header">Select node</h1>
      <div class="chart-menu">
        <div class="menu-item" v-for="(item, index) in items" v-bind:key="index">
          <label class="menu-item-label">{{item}}</label>
          <input type="radio" :value="index" v-model="currentNode" v-on:change="onNodeChange">
        </div>
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

.chart-container {
  text-align: center;
}

.chart-menu {
  display: inline-block;
  text-align: center;
  border: 1px solid black;
  padding: 10px 20px;
}

.menu-item {
  display: inline-block;
  width: 200px;
  font-size: 22px;
}

.main-header {
  font-size: 28px;
  margin: 10px 0;
}
</style>