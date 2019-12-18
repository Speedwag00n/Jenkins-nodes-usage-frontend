<template>
  <div id="app">
    <NodesTable :nodes="nodesNames" :nodesStats="stats"/>

    <Chart :chartData="chartData"/>
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

    chartData: [
      ['Date', 'In use'],
      ['2019-12-15', 8],
      ['2019-12-14', 7],
      ['2019-12-13', 6],
      ['2019-12-12', 4],
      ['2019-12-11', 9],
      ['2019-12-10', 12],
      ['2019-12-09', 1]
    ]
  }),
  methods: {
    getStringDate: function(date) {
      let day = String(date.getDate()).padStart(2, '0');
      let mouth = String(date.getMonth() + 1).padStart(2, '0');
      let year = date.getFullYear();
      return year + "-" + mouth + "-" + day;
    }
  },
  created() {
    let date = new Date();
    let todayString = this.getStringDate(date);
    date = new Date(date.getTime() - (60*60*24*6*1000)); //week ago
    let weekAgoString = this.getStringDate(date);
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