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
        columns: ['API','count'],
        rows: []
      }
    }
  },
  mounted(){
    axios.get('http://localhost:8080/questions/javaAPIFrequency').then( (resp)=> {
      this.chartData.rows = resp.data.map(obj => ({
        API: obj[0],
        count: obj[1]
      }));

      console.log( this.chartData);
      Vue.set(this, 'chartData', this.chartData);
    });
  }
}
</script>
