<template>
  <div id="app" class="columns">
    <ScatterChart class="column" />
    <BarChart class="column" />
    <GraphChart class="column" />
  </div>
</template>

<script>
import ScatterChart from './components/ScatterChart.vue'
import BarChart from "./components/BarChart";
import GraphChart from "./components/GraphChart";

export default {
  name: 'App',
  components: {
    ScatterChart,
    BarChart,
    GraphChart
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
