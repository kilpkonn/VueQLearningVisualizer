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
          text: 'Buckets'
        },
        tooltip: {},
        xAxis: {
          data: [0]
        },
        yAxis: {},
        series: [{
          name: 'Results',
          type: 'bar',
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
      async loadData() {
        fetch("/data/rod_angles.json")
          .then((res) => res.json())
          .then((json) => this.jsonData = json)
      },
      async showData(n) {
        if (!this.jsonData) return;
        const that = this;
        that.bar.xAxis.data = this.jsonData[n].rod_angles;
        that.bar.series[0].data = this.jsonData[n].rod_angles_values;
      }
    }
  };
</script>

<style scoped>
    .echarts {
        height: 500px;
    }
</style>
