<template>
    <div class="echarts">
        <IEcharts
                :option="scatter"
                :loading="loading"
                @ready="onReady"
                @click="onClick"
        />
        <button @click="doRandom">Random</button>
    </div>
</template>

<script type="text/babel">
  import IEcharts from 'vue-echarts-v3/src/full.js';
  import * as ecStat from "echarts-stat";

  export default {
    name: 'scatter',
    components: {
      IEcharts
    },
    props: {},
    data: () => ({
      loading: false,
      scatter: {
        title: {
          text: 'Simulation results'
        },
        tooltip: {},
        xAxis: {
          data: [0]
        },
        yAxis: {},
        series: [{
          name: 'Results',
          type: 'scatter',
          data: [0],
        },
          {
            name: 'line',
            type: 'line',
            smooth: true,
            showSymbol: false,
            data: [0],
            markPoint: {
              itemStyle: {
                color: 'transparent'
              },
              label: {
                show: true,
                position: 'left',
                formatter: null,
                color: '#333',
                fontSize: 12
              },
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
      doRandom() {
        const that = this;
        let data = [];
        for (let i = 0, min = 5, max = 99; i < 6; i++) {
          data.push(Math.floor(Math.random() * (max + 1 - min) + min));
        }
        that.loading = !that.loading;
        that.scatter.series[0].data = data;
      },
      onReady(instance, ECharts) {
        console.log(instance, ECharts);
      },
      onClick(event, instance, ECharts) {
        console.log(event, instance, ECharts);
      },
      async loadData() {
        fetch("/data/result_list.json")
          .then((res) => res.json())
          .then((res) => {
            const that = this;
            let xData = [];
            let regData = [];
            for (let i = 0; i < res.length; i++) {
              xData.push(i);
              regData.push([i, res[i]])
            }
            that.scatter.xAxis.data = xData;
            that.scatter.series[0].data = res;

            const myRegression = ecStat.regression('polynomial', regData, 3);

            myRegression.points.sort(function (a, b) {
              return a[0] - b[0];
            });
            that.scatter.series[1].data = myRegression.points;

            that.scatter.series[1].markPoint.data = [{
              coord: myRegression.points[myRegression.points.length - 1]
            }];
            that.scatter.series[1].markPoint.label.formatter = myRegression.expression;
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
