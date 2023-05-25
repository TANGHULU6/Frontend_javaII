<template>
  <div>
    <p class="warn-content">
      <a href="https://v-charts.js.org/#/" target="_blank">最活跃的用户
      </a>
    </p>
    <ve-histogram
      :data="chartData"
                  :tooltip="chartDataTooltip"></ve-histogram>
  </div>

</template>

<script>
import axios from "axios";
import Vue from "vue";

export default {
  data() {
    let that=this;
    return {
      chartData: {
        columns: ['id','user_answer_cnt','user_comment_cnt'],
        rows: [],
      },
      chartDataTooltip: {
          formatter: function (data) {
            return `${that.rowProperties[data.name].question_id}<br>(most_active_answer_user)${that.rowProperties[data.name].most_active_answer_user}: ${that.rowProperties[data.name].user_answer_cnt}<br>(most_active_comment_user)${that.rowProperties[data.name].most_active_comment_user}: ${that.rowProperties[data.name].user_comment_cnt}`;
          }
        },

      rowProperties: [],


    }
  },
  mounted() {
    axios.get('http://localhost:8080/questions/mostActiveUsersPerQuestion').then((resp) => {
      const rows = resp.data.map((obj, index) => ({
        id: index + 1,
        question_id: obj[0],
        most_active_answer_user: obj[1],
        user_answer_cnt: obj[2],
        most_active_comment_user: obj[3],
        user_comment_cnt: obj[4]
      }));

      this.chartData.rows = rows;
      this.rowProperties = rows;

      console.log(this.chartData);
      Vue.set(this, 'chartData', this.chartData);
    });
  },

  // methods: {
  //   async getMostActiveAnswerUser(userId) {
  //     try {
  //       const response = await axios.get(`http://localhost:8080/questions/mostActiveUsersPerQuestion`);
  //       return response.data.most_active_answer_user;
  //     } catch (error) {
  //       console.error(error);
  //       return 'N/A'; // 如果请求出错，返回默认值
  //     }
  //   },
  //   async getMostActiveCommentUser(userId) {
  //     try {
  //       const response = await axios.get(`http://localhost:8080/users/questions/mostActiveUsersPerQuestion`);
  //       return response.data.most_active_comment_user;
  //     } catch (error) {
  //       console.error(error);
  //       return 'N/A'; // 如果请求出错，返回默认值
  //     }
  //   }
  // }
}
</script>
