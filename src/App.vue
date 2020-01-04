<template>
  <div id="app">
    <NodesTable :nodesStats="stats" :dates="dates"/>

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
    stats: [],
    nodesNames: [],
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

    changeChart: function(index) {
      let currentStat = this.stats[index].usages;
      let newData = [];
      newData.push(['Data', 'In use']);
      currentStat.forEach(
        function(item) {
          newData.push([item.date, item.duration]);
        }
      );
      this.chartData = newData;
    }
  },
  created() {
    this.generateDatesForWeek();

    let todayString = this.dates[this.dates.length - 1];
    let weekAgoString = this.dates[0];

    let nodesNames = this.nodesNames;

    axios
      .get('http://localhost:5000/api/working/usage/period' + '?stop_date=' + todayString + '&start_date=' + weekAgoString)
      .then(
        response => {
          this.stats = response.data;
          this.stats.forEach(
            function(item) {
              nodesNames.push(item.node_name);
            }
          )
        }
      );
  }
}
</script>