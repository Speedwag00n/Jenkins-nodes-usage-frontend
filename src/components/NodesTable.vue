<template>
  <div class="table-container">
    <table class="node-table">
      <tr>
        <th>Node</th>
        <th v-for="(item, index) in nodesStats[0]" v-bind:key="index">{{item.date}}</th>
      </tr>
      <tr v-for="(stats, statsIndex) in nodesStats" v-bind:key="statsIndex">
        <td>{{nodes[statsIndex].name}}</td>
        <td v-for="(item, itemIndex) in stats" v-bind:key="itemIndex">{{Math.round(item.duration)}}</td> <!--in seconds-->
      </tr>
    </table>
  </div>
</template>

<script>

export default {
  name: 'NodesTable',
  props: ['nodes', 'nodesStats'],
  components: {

  },
  data: () => ({
    weekDays: ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
  })
}
</script>

<style scoped>
.table-container {
  text-align: center;
}

.node-table{
  font-size: 18px;
  margin: 20px 10px;
	border-collapse: collapse;
}

.node-table th,
.node-table td{
  padding: 5px;
  min-width: 200px;
	text-align: center;
	border: none;
	border: 2px solid black;
	vertical-align: middle;
	text-align: center;
}
</style>