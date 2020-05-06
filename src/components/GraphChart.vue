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
        let nodes = [];
        let edges = [];
        for (let i = 0; i < n; i++) {
          if (!this.jsonData[i]) continue;

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
          for (const node of nodes) {
            if (that.graph.series[0].data.filter(a => a.id === node.id).length === 0) {
              that.graph.series[0].data.push(node);
            }
          }
        }
        if (that.graph.series[0].links.length !== edges.length) {
          for (const edge of edges) {
            if (that.graph.series[0].links.filter(e => e.target === edge.target && e.source === edge.source)) {
              that.graph.series[0].links.push(edge);
            }
          }
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
