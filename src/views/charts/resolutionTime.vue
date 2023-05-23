<template>
    <ve-bar :data="chartData"></ve-bar>
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
