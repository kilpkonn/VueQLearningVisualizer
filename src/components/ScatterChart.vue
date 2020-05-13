<template>
    <div class="echarts">
        <IEcharts
                :option="scatter"
                :loading="loading"
                @ready="onReady"
                @click="onClick"
        />
    </div>
</template>

<script type="text/babel">
  import IEcharts from 'vue-echarts-v3/src/full.js';
  import * as ecStat from "echarts-stat";
  import { LinearGradient } from "echarts/lib/util/graphic";

  export default {
    name: 'scatter',
    components: {
      IEcharts
    },
    props: {},
    data: () => ({
      loading: false,
      jsonData: {},
      scatter: {
        title: {
          text: 'Simulation results',
          subtext: 'Dots represent simulation result scores (bigger better)',
          textStyle: {
            color: "#d4d4d4"
          },
          subtextStyle: {
            color: "#a7a7a7"
          },
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
        yAxis: {
          min: 0,
          max: 200
        },
        series: [{
          name: 'Results',
          type: 'scatter',
          color:  new LinearGradient(0.1, 0, 0, 1, [{
            offset: 0,
            color: '#73ebfc'
          }, {
            offset: 1,
            color: '#4b85ff'
          }]),
          data: [0],
        },
          {
            name: 'line',
            type: 'line',
            color:  new LinearGradient(0.5, 0.5, 0.4, 1, [{
              offset: 0,
              color: '#e2c9ff'
            }, {
              offset: 1,
              color: '#c945ff'
            }]),
            smooth: true,
            showSymbol: false,
            data: [0],
            markPoint: {
              itemStyle: {
                color: 'transparent'
              },
              /*label: {
                show: true,
                position: 'left',
                formatter: null,
                color: '#333',
                fontSize: 12
              },*/
              data: [{
                coord: []
              }]
            }
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
        fetch("./data/result_list.json")
          .then((res) => res.json())
          .then((json) => this.jsonData = json)
      },
      showData(n) {
        if (!this.jsonData) return;

        const that = this;
        let xData = [];
        let regData = [];
        for (let i = 0; i < n % this.jsonData.length; i++) {
          xData.push(i);
          regData.push([i, this.jsonData[i]])
        }
        that.scatter.xAxis.data = xData;
        that.scatter.series[0].data = this.jsonData.slice(0, n % this.jsonData.length);

        const myRegression = ecStat.regression('polynomial', regData, 4);

        myRegression.points.sort(function (a, b) {
          return a[0] - b[0];
        });
        that.scatter.series[1].data = myRegression.points;

        that.scatter.series[1].markPoint.data = [{
          coord: myRegression.points[myRegression.points.length - 1]
        }];
        // that.scatter.series[1].markPoint.label.formatter = myRegression.expression;
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
