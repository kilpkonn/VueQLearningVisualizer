<template>
    <div class="echarts">
        <IEcharts
                :option="line"
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
    name: 'lineChart',
    components: {
      IEcharts
    },
    props: {},
    data: () => ({
      loading: false,
      jsonData: {},
      line: {
        title: {
          text: 'Alpha & Epsilon vs Avg move',
          textStyle: {
            color: "#d4d4d4"
          }
        },
        tooltip: {},
        xAxis: {
          data: [0]
        },
        axisLabel: {
          fontWeight: 'bolder',
          color: '#b6b6b6',
          shadowColor: '#c6c5c5',
          shadowBlur: 10
        },
        yAxis: {},
        series: [{
          name: 'Results',
          type: 'line',
          data: [0],
          color:  new LinearGradient(0.5, 0.5, 0.4, 1, [{
            offset: 0,
            color: '#1f7bff'
          }, {
            offset: 1,
            color: '#d7e8fc'
          }]),
        },
          {
            name: 'Alpha',
            type: 'line',
            data: [0],
            color:  new LinearGradient(0.5, 0.5, 0.4, 1, [{
              offset: 0,
              color: '#5b1fff'
            }, {
              offset: 1,
              color: '#b665fd'
            }]),
          },
          {
            name: 'Epsilon',
            type: 'line',
            data: [0],
            color:  new LinearGradient(0.5, 0.5, 0.4, 1, [{
              offset: 0,
              color: '#9a0707'
            }, {
              offset: 1,
              color: '#c77e31'
            }]),
          },
        ]
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
        fetch("./data/avg_move_list.json")
          .then((res) => res.json())
          .then((json) => this.jsonData = json)
      },
      showData(n) {
        if (!this.jsonData || !this.jsonData.move) return;
        const that = this;

        let xData = [];
        for (let i = 0; i <= n % this.jsonData.move.length; i++) {
          xData.push(i);
        }

        that.line.xAxis.data = xData;
        that.line.series[0].data = this.jsonData.move.slice(0, n % this.jsonData.move.length);
        that.line.series[1].data = this.jsonData.alpha.slice(0, n % this.jsonData.move.length);
        that.line.series[2].data = this.jsonData.epsilon.slice(0, n % this.jsonData.move.length);
      }
    }
  };
</script>

<style scoped>
    .echarts {
        height: 490px;
    }
</style>
