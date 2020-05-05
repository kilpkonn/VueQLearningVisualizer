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
      jsonData: {},
      graph: {
        title: {
          text: 'Q-nodes'
        },
        tooltip: {},
        series: [{
          type: 'graph',
          layout: 'force',
          animation: false,
          data: [],
          force: {
            // initLayout: 'circular'
            // gravity: 0
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
      showData(n) {
        if (!this.jsonData) return;
        const that = this;
        let nodes = [];
        let edges = [];
        for (let i = 0; i < n; i++) {
          this.jsonData[i].nodes.forEach(node => {
            if (nodes.filter(no => no.id === node).length === 0) {
              nodes.push({
                symbolSize: 10,
                id: node,
                name: node
              });
            }
          });

          this.jsonData[i].edges.forEach(e => {
            edges.push({
              source: e.source,
              target: e.target,
              lineStyle: {
                width: 1,
                curveness: 0.0
              }
            });
          });
        }
        if (that.graph.series[0].data.length !== nodes.length) {
          that.graph.series[0].data = nodes;
        }
        if (that.graph.series[0].links.length !== edges.length) {
          that.graph.series[0].links = edges;
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
