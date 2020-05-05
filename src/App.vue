<template>
  <div id="app" class="columns">
    <ScatterChart class="column" />
    <BarChart ref="barChart" class="column" />
    <GraphChart class="column" />
    <LineChart class="column" />
  </div>
</template>

<script>
import ScatterChart from './components/ScatterChart.vue'
import BarChart from "./components/BarChart";
import GraphChart from "./components/GraphChart";
import LineChart from "./components/LineChart";

export default {
  name: 'App',
  components: {
    ScatterChart,
    BarChart,
    GraphChart,
    LineChart
  },
  mounted() {
    setInterval(() => this.updateCharts(), 1000);
  },
  methods: {
    updateCharts() {
      this.$refs.barChar.showData(2)
    }
  }
}
</script>

<style lang="scss">
  @import "../node_modules/bulma/bulma.sass";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
