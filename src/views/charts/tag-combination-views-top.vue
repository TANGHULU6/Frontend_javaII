<template>
  <div>
    <p class="warn-content">
      <a href="https://v-charts.js.org/#/" target="_blank">tag组合的查看数（前十）
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
        columns: ['id','count'],
        rows: []
      }
    }
  },
  mounted(){
    axios.get('http://localhost:8080/questionTags/tagCombinationViews').then( (resp)=> {
      const maxDataPoints = 10;

      let data = resp.data;

      if (data.length > maxDataPoints) {
        data = data.slice(0, maxDataPoints);
      }
      this.chartData.rows = data.map(obj => ({
        id: obj[0],
        count: obj[1]
      }));

      console.log( this.chartData);
      Vue.set(this, 'chartData', this.chartData);
    });
  }
}
</script>
