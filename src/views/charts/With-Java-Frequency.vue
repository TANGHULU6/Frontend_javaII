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
        columns: ['id','count'],
        rows: []
      }
    }
  },
  mounted(){
    axios.get('http://localhost:8080/questionTags/tagWithJavaFrequency').then( (resp)=> {
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
