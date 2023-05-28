<template>
  <div>
    <p class="warn-content">
      <a href="https://v-charts.js.org/#/" target="_blank">展示问题从提出到解决 (answer accepted time – question post time) 的时间间隔分布
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
        columns: ['duration','count'],
        rows: []
      }
    }
  },
  mounted() {
    axios.get('http://localhost:8080/questions/questionResolutionTimeDistribution').then((resp) => {
      const data = resp.data;
      const interval = 10000; // 设置区间的大小，这里假设为10

      // 将数据按区间进行分组
      const groupedData = data.reduce((result, obj) => {
        const duration = obj[0];
        const count = obj[1];

        // 计算所属的区间
        const intervalIndex = Math.floor(duration / interval);
        const intervalStart = intervalIndex * interval;
        const intervalEnd = (intervalIndex + 1) * interval;

        // 添加到对应的区间
        if (!result[intervalStart]) {
          result[intervalStart] = { duration: intervalStart + '-' + intervalEnd, count: 0 };
        }
        result[intervalStart].count += count;

        return result;
      }, {});

      // 将分组后的数据转为数组形式，用于绘制柱状图
      const groupedDataArray = Object.values(groupedData);

      this.chartData.rows = groupedDataArray.map(obj => ({
        duration: obj.duration,
        count: obj.count
      }));

      console.log(this.chartData);
      Vue.set(this, 'chartData', this.chartData);
    });
  }

}
</script>
