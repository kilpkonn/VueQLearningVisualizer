<template>
  <div id="app" class="columns">
    <ScatterChart ref="scatterChart" class="column" />
    <BarChart ref="barChart" class="column" />
    <GraphChart ref="graphChart" class="column" />
    <LineChart ref="lineChart" class="column" />
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
  data: () => ({
    n: 0
  }),
  mounted() {
    setInterval(() => this.updateCharts(), 300);
  },
  methods: {
    updateCharts() {
      this.n++;
      this.$refs.barChart.showData(this.n);
      this.$refs.scatterChart.showData(this.n);
      this.$refs.lineChart.showData(this.n);
      this.$refs.graphChart.showData(this.n);
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
