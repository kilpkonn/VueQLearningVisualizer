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
          text: 'aaa'
        },
        tooltip: {},
        xAxis: {
          data: [0]
        },
        yAxis: {},
        series: [{
          name: 'Results',
          type: 'line',
          data: [0],
        },
          {
            name: 'Alpha',
            type: 'line',
            data: [0],
          },
          {
            name: 'Epsilon',
            type: 'line',
            data: [0],
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
      async loadData() {
        fetch("/data/avg_move_list.json")
          .then((res) => res.json())
          .then((json) => this.jsonData = json)
      },
      async showData(n) {
        if (!this.jsonData) return;

        const that = this;

        let xData = [];
        for (let i = 0; i <= n; i++) {
          xData.push(i);
        }

        that.line.xAxis.data = xData;
        that.line.series[0].data = this.jsonData.move.slice(0, n);
        that.line.series[1].data = this.jsonData.alpha.slice(0, n);
        that.line.series[2].data = this.jsonData.epsilon.slice(0, n);
      }
    }
  };
</script>

<style scoped>
    .echarts {
        height: 500px;
    }
</style>
