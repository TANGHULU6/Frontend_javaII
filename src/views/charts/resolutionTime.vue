<template>
  <div>
    <p class="warn-content">
      <a href="https://v-charts.js.org/#/" target="_blank">展示问题从提出到解决 (answer accepted time – question post time) 的时间间隔分布
      </a>
    </p>
    <ve-bar :data="chartData"></ve-bar>
  </div>
</template>

<script>
import axios from "axios";
import Vue from "vue";

export default {
  data() {
    return {
      chartData: {
        columns: ['duration','count'],
        rows: []
      }
    }
  },
  mounted(){
    axios.get('http://localhost:8080/questions/questionResolutionTimeDistribution').then( (resp)=> {
      this.chartData.rows = resp.data.map(obj => ({
        duration: obj[0],
        count: obj[1]
      }));

      console.log( this.chartData);
      Vue.set(this, 'chartData', this.chartData);
    });
  }
}
</script>
