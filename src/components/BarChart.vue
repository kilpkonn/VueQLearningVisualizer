<template>
    <div class="echarts">
        <IEcharts
                :option="bar"
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
    name: 'bar',
    components: {
      IEcharts
    },
    props: {},
    data: () => ({
      loading: false,
      jsonData: {},
      bar: {
        title: {
          text: 'Buckets distribution (logarithmic)',
          textStyle: {
            color: "#d4d4d4"
          }
        },
        tooltip: {},
        xAxis: {
          data: [0],
        },
        axisLabel: {
          fontWeight: 'bolder',
          color: '#b6b6b6',
          shadowColor: '#c6c5c5',
          shadowBlur: 10
        },
        yAxis: {
          type: 'log',
          min: 1
        },
        series: [{
          name: 'Results',
          type: 'bar',
          color:  new LinearGradient(0.5, 0.5, 0.4, 1, [{
            offset: 0,
            color: '#1f7bff'
          }, {
            offset: 1,
            color: '#d7e8fc'
          }]),
          data: [0],
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
        fetch("/data/rod_angles.json")
          .then((res) => res.json())
          .then((json) => this.jsonData = json)
      },
      showData(n) {
        if (!this.jsonData[n % this.jsonData.length]) return;

        const that = this;
        that.bar.xAxis.data = this.jsonData[n % this.jsonData.length].rod_angles;
        that.bar.series[0].data = this.jsonData[n % this.jsonData.length].rod_angles_values;
      }
    }
  };
</script>

<style scoped>
    .echarts {
        height: 480px;
    }
</style>
