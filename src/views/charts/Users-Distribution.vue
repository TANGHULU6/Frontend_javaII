<template>
  <div>
  <p class="warn-content">
    <a href="https://v-charts.js.org/#/" target="_blank">参与讨论的用户分布
    </a>
  </p>
    <ve-histogram :data="chartData"></ve-histogram>
  </div>
</template>

<script>
import axios from "axios";
import Vue from "vue";

export default {
  data() {
    return {
      chartData: {
        columns: ['id','total_user_count','answer_user_count','comment_user_count'],
        rows: []
      }
    }
  },
  mounted(){
    axios.get('http://localhost:8080/questions/countUniqueUsersPerQuestion').then( (resp)=> {
      this.chartData.rows = resp.data.map(obj => ({
        id: obj[0],
        count: obj[1],
        answer_user_count: obj[2],
        comment_user_count: obj[3]
      }));

      console.log( this.chartData);
      Vue.set(this, 'chartData', this.chartData);
    });
  }
}
</script>
