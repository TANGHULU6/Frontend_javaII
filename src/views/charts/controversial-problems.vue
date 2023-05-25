<template>
  <div>
    <p class="warn-content">
      <a href="https://v-charts.js.org/#/" target="_blank">含有 non-accepted answer 的 upvote 数高于 accepted answer 的问题的百分比
      </a>
    </p>
  <ve-pie :data="chartData"></ve-pie>
  </div>
</template>


<script>
import axios from "axios";
import Vue from "vue";

export default {
  data() {
    return {
      chartData: {
        columns: [],
        rows: []
      }
    }
  },
  mounted(){
    axios.get('http://localhost:8080/questions/countQuestionsWithLowerAcceptedAnswerScore').then( (resp)=> {
      this.chartData.columns = ['category', 'count'];
      this.chartData.rows = [
        { category: 'ac answer has more upvotes', count: 600-resp.data },
        { category: 'not ac answer has more upvotes', count: resp.data }
      ];


      console.log( this.chartData);
      Vue.set(this, 'chartData', this.chartData);
    });
  }
}
</script>
