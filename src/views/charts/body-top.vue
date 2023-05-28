<template>
  <div>
  <p class="warn-content">
    <a href="https://v-charts.js.org/#/" target="_blank">API TOP10
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
        columns: ['API','count'],
        rows: []
      }
    }
  },
  mounted(){
    axios.get('http://localhost:8080/questions/javaAPIFrequency').then( (resp)=> {
      const maxDataPoints = 10;

      let data = resp.data;

      if (data.length > maxDataPoints) {
        data = data.slice(0, maxDataPoints);
      }
      this.chartData.rows = data.map(obj => ({
        API: obj[0],
        count: obj[1]
      }));

      console.log( this.chartData);
      Vue.set(this, 'chartData', this.chartData);
    });
  }
}
</script>
