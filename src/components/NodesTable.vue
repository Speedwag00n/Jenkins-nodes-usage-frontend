<template>
  <div class="table-container">
    <table class="node-table">
      <tr>
        <th>Node</th>
        <th v-for="(day, index) in weekDays" v-bind:key="index">{{day}}</th>
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

</style>