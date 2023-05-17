<template>
  <ve-pie :data="chartData"></ve-pie>
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
  mounted() {
    axios.get('http://localhost:8080/questions').then(resp => {

      const groupedData = {
        withAcceptedAnswer: [],
        withoutAcceptedAnswer: []
      };

      resp.data.forEach(item => {
        if (item.acceptedAnswerId != null) {
          groupedData.withAcceptedAnswer.push(item);
        } else {
          groupedData.withoutAcceptedAnswer.push(item);
        }
      });

      this.chartData.columns = ['category', 'count'];
      //let sum=groupedData.withAcceptedAnswer.length+groupedData.withoutAcceptedAnswer.length;
      this.chartData.rows = [
        { category: 'have Accepted Answer', count: groupedData.withAcceptedAnswer.length },
        { category: 'not have Accepted Answer', count: groupedData.withoutAcceptedAnswer.length }
      ];

      Vue.set(this, 'chartData', this.chartData);
    });
  }
}
</script>
