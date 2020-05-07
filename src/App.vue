<template>
    <div id="app">
        <div class="is-centered">
            <h1 class="is-size-1">Q learning</h1>
        </div>
        <div class="columns graph">
            <GraphChart ref="graphChart" class="column"/>
        </div>
        <div class="columns secondary-charts-container">
            <ScatterChart ref="scatterChart" class="column"/>
            <BarChart ref="barChart" class="column"/>
            <LineChart ref="lineChart" class="column"/>
        </div>
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
      setTimeout(() => {
        setInterval(() => this.updateCharts(), 300);
      }, 1000);
    },
    methods: {
      updateCharts() {
        this.$refs.barChart.showData(this.n);
        this.$refs.scatterChart.showData(this.n);
        this.$refs.lineChart.showData(this.n);
        this.$refs.graphChart.showData(this.n);
        this.n++;
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
        color: #ececec;
        margin-top: 0;
        background: #000f2a;

        .graph {
            position: absolute;
            top: 20px;
            right: 20px;
            left: 20px;
            height: 39vh;
        }

        .secondary-charts-container {
            margin-top: 40vh;
            margin-bottom: 0;
            background: transparent;
        }
    }

</style>
