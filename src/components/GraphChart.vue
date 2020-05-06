<template>
    <div class="echarts">
        <IEcharts
                :option="graph"
                :loading="loading"
                @ready="onReady"
                @click="onClick"
        />
        <!-- <button @click="doRandom">Random</button> -->
    </div>
</template>

<script type="text/babel">
  import IEcharts from 'vue-echarts-v3/src/full.js';
  // import * as ecStat from "echarts-stat";

  export default {
    name: 'graph',
    components: {
      IEcharts
    },
    props: {},
    data: () => ({
      loading: false,
      jsonData: [],
      graph: {
        /*title: {
          text: 'Q-nodes'
        },*/
        tooltip: {},
        series: [{
          type: 'graph',
          layout: 'force',
          animation: true,
          animationDuration: 300,
          animationEasing: 'cubicInOut',
          roam: true,
          data: [],
          force: {
            initLayout: 'circular',
            layoutAnimation: true,
            gravity: 0,
            repulsion: 100,
            edgeLength: 5
          },
          links: []
        }]
      }
    }),
    async mounted() {
      this.loadData();
    },
    methods: {
      onReady(instance, ECharts) {
        console.log(instance, ECharts);
      },
      onClick(event, instance, ECharts) {
        console.log(event, instance, ECharts);
      },
      loadData() {
        fetch("/data/nodes.json")
          .then((res) => res.json())
          .then((json) => this.jsonData = json);
      },
      async showData(n) {
        if (!this.jsonData) return;

        const that = this;
        n = n % this.jsonData.length;
        if (n === 0) {
          that.graph.series[0].data = [];
          that.graph.series[0].links = [];
        }

        for (const nde of this.jsonData[n].nodes) {
          const node = {
            symbolSize: 10,
            id: nde,
            name: nde
          };

          that.graph.series[0].data.push(node);
        }

        for (const e of this.jsonData[n].edges) {
          const edge = {
            source: e.source,
            target: e.target,
            lineStyle: {
              width: 1,
              curveness: 0.0
            }
          };
          that.graph.series[0].links.push(edge);
        }
      }
    }
  };
</script>

<style scoped>
    .echarts {
        height: 500px;
    }
</style>
