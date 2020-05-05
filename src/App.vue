<template>
  <div id="app">
    <ScatterChart />
  </div>
</template>

<script>
import ScatterChart from './components/ScatterChart.vue'

export default {
  name: 'App',
  components: {
    ScatterChart
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
