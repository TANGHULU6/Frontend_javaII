<template>
  <div>
    <p class="warn-content">
      <a href="https://v-charts.js.org/#/" target="_blank">问题回答数分布
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
        columns: ['questionId','answerCount'],
        rows: []
      },
      chartExtend: {
        xAxis: {
          axisLabel: {
            interval: 0,
            formatter: function(value) {
              return value.split("").join("\n");
            }
          }
        }
      }
    }
  },
  mounted(){
    axios.get('http://localhost:8080/questions').then( (resp)=> {
      this.chartData.rows= resp.data

      console.log( this.chartData);
      Vue.set(this, 'chartData', this.chartData);
    });
  }
}
</script>
