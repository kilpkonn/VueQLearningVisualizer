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
      graph: {
        title: {
          text: 'Q-nodes'
        },
        tooltip: {},
        series: [{
          type: 'graph',
          layout: 'force',
          animation: false,
          data: [0],
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
      async loadData() {
        fetch("/data/nodes.json")
          .then((res) => res.json())
          .then((json) => {
            const that = this;
            let nodes = json.nodes;
            nodes = nodes.map(n => {
              return {
                symbolSize: 10,
                id: n,
                name: n
              }
            });
            json.edges = json.edges.map(e => {
              return {
                source: e.source,
                target:e.target,
                lineStyle: {
                  width: 1,
                  curveness: 0.0
                }
              }
            });
            that.graph.series = [{
              roam: true,
              data: nodes,
              links: json.edges
            }]
          })

      }
    }
  };
</script>

<style scoped>
    .echarts {
        height: 500px;
    }
</style>
