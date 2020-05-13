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
  import { LinearGradient } from "echarts/lib/util/graphic";
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
        title: {
          text: 'Q-nodes',
          subtext: 'Nodes represent states (think of them as position coordinates), links are actions (transitions from state to another)',
          textStyle: {
            color: "#d4d4d4"
          },
          subtextStyle: {
            color: "#a7a7a7"
          },
        },
        tooltip: {},
        series: [{
          type: 'graph',
          layout: 'force',
          animation: true,
          animationDuration: 500,
          animationEasing: 'linear',
          color:  new LinearGradient(0.5, 0.5, 0.4, 1, [{
            offset: 0,
            color: '#dd39ff'
          }, {
            offset: 1,
            color: '#ffffff'
          }]),
          roam: true,
          data: [],
          force: {
            initLayout: 'circular',
            layoutAnimation: true,
            gravity: 0.01,
            repulsion: 100,
            edgeLength: 10
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
        fetch("./data/nodes.json")
          .then((res) => res.json())
          .then((json) => this.jsonData = json);
      },
      showData(n) {
        if (!this.jsonData) return;

        const that = this;
        n = n % this.jsonData.length;
        if (n === 0) {
          that.graph.series[0].data = [];
          that.graph.series[0].links = [];
        }

        for (const nde of this.jsonData[n].nodes) {
          const node = {
            symbolSize: 9,
            id: nde,
            name: nde,
          };

          that.graph.series[0].data.push(node);
        }

        for (const e of this.jsonData[n].edges) {
          const edge = {
            source: e.source,
            target: e.target,
            lineStyle: {
              width: 2,
              curveness: 0.0,
              color:  new LinearGradient(0.5, 0.5, 0.4, 1, [{
                offset: 0,
                color: '#6ca8ff'
              }, {
                offset: 1,
                color: '#f1f6ff'
              }]),
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
        height: 420px;
        margin: 0;
    }
</style>
