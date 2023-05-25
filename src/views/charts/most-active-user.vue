<template>
  <div>
    <p class="warn-content">
      <a href="https://v-charts.js.org/#/" target="_blank">最活跃的用户
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
    axios.get('http://localhost:8080/mostActiveUsersPerQuestion').then( (resp)=> {
      this.chartData.rows = resp.data.map(obj => ({
        id: obj[0],
        count: obj[1]
      }));

      console.log( this.chartData);
      Vue.set(this, 'chartData', this.chartData);
    });
  }
}
</script>
