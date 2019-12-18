<template>
  <div id="app">
    <NodesTable :nodes="nodesNames" :nodesStats="stats" :dates="dates"/>

    <Chart :items="nodesNames" :chartData="chartData" v-on:nodechange="changeChart"/>
  </div>
</template>

<script>
import Chart from './components/Chart.vue'
import NodesTable from './components/NodesTable.vue'

import axios from 'axios'

export default {
  name: 'app',
  components: {
    Chart,
    NodesTable
  },
  data: () => ({
    nodesNames: [],
    stats: [],
    currentNode: '',
    dates: [],

    chartData: []
  }),
  methods: {
    getStringDate: function(date) {
      let day = String(date.getDate()).padStart(2, '0');
      let mouth = String(date.getMonth() + 1).padStart(2, '0');
      let year = date.getFullYear();
      return year + "-" + mouth + "-" + day;
    },

    generateDatesForWeek() {
      let date = new Date();
      date = new Date(date.getTime() - (60 * 60 * 24 * 1000 * 6));
      for (let i = 0; i < 7; i ++) {
        this.dates.push(this.getStringDate(date));
        date = new Date(date.getTime() + (60 * 60 * 24 * 1000 * 1));
      }
    },

    changeChart: function(value) {
      this.currentNode = value;
      let index = 0;
      for (let i = 0; i < this.nodesNames.length; i++) {
        if (value === this.nodesNames[i].name) {
          index = i;
          break;
        }
      }
      this.updateChartData(index);
    },

    updateChartData: function(index) {
      let currentStat = this.stats[index];
      let newData = [];
      newData.push(['Data', 'In use']);
      currentStat.forEach(function(item) {
        newData.push([item.date, item.duration]);
      });
      this.chartData = newData;
    }
  },
  created() {
    this.generateDatesForWeek();

    let todayString = this.dates[this.dates.length - 1];
    let weekAgoString = this.dates[0];

    let stats = this.stats
    axios
      .get('http://localhost:5000/api/node')
      .then(
        response => {
          this.nodesNames = response.data
          this.nodesNames.forEach(function(node) {
            axios
              .get('http://localhost:5000/api/working/usage/period/' + node.name + '?stop_date=' + todayString + '&start_date=' + weekAgoString)
              .then(
                response => {
                  stats.push(response.data)
                }
              )
          });
        }
      );
  }
}
</script>