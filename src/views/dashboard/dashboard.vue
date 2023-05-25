<template>
  <div class="app-container">
   <nx-github-corner></nx-github-corner>
     <!-- <a target="_blank" href="https://github.com/mgbq/vue-permission"><img
        style="position: absolute; top: 48px; right: 0; border: 0;"
        src="../../../static/img/report/forkme.png"
        alt="Fork me on GitHub"></a> -->
    <div class="item">
      <h4>数据展示</h4>
      <nx-data-display :option="option"></nx-data-display>
    </div>
    <div class="item">
      <h4>数据分析</h4>
      <nx-data-tabs :option="easyDataOption"></nx-data-tabs>
    </div>
<!--    <div class="item">-->
<!--      <h4>卡片的展示</h4>-->
<!--      <nx-data-card :option="easyDataOption0"></nx-data-card>-->
<!--    </div>-->
<!--    <div class="item">-->
<!--      <h4>带数字的展示</h4>-->
<!--      <nx-data-icons :option="easyDataOption1"></nx-data-icons>-->
<!--    </div>-->
    <div class="item">
      <h4>简易展示</h4>
      <nx-data-icons :option="easyDataOption2"></nx-data-icons>
    </div>
  </div>
</template>

<script>

import nxDataDisplay from '@/components/nx-data-display/nx-data-display'
import nxDataCard from '@/components/nx-data-card/nx-data-card'
import nxDataTabs from '@/components/nx-data-tabs/nx-data-tabs'
import nxDataIcons from '@/components/nx-data-icons/nx-data-icons'
import nxGithubCorner from '@/components/nx-github-corner'
import axios from "axios";
export default {
  name: 'report',
  components: {
    nxDataDisplay,
    nxDataCard,
    nxDataTabs,
    nxDataIcons,
    nxGithubCorner

  },
  data() {
    return {
      option: {
        span: 8,
        color: '#15A0FF',
        data: [
          {
            count: 600,
            title: '分析数据'
          },
          {
            count: 600,
            title: '爬取数据'
          }
          // {
          //   count: 20000,
          //   title: '年活跃数'
          // }
        ]
      },
      easyDataOption: {
        span: 6,
        data: [
          {
            title: '最热门问题收获回答',
            subtitle: '实时',
            count: 0,
            allcount: 3000,
            text: '没有回答的问题数',
            color: 'rgb(49, 180, 141)',
            key: '问'
          },
          {
            title: '平均一个问题收获回答',
            subtitle: '实时',
            count: 0,
            allcount: 0,
            text: '精确值',
            color: 'rgb(56, 161, 242)',
            key: '均'
          },
          {
            title: '活跃用户',
            subtitle: '实时',
            count: 0,
            allcount: 10222,
            text: '最活跃用户',
            color: 'rgb(117, 56, 199)',
            key: '水'
          },
          // {
          //   title: '新闻统计',
          //   subtitle: '实时',
          //   count: 908,
          //   allcount: 10222,
          //   text: '评论次数',
          //   color: 'rgb(59, 103, 164)',
          //   key: '新'
          // }
        ]
      },
      // easyDataOption0: {
      //   span: 6,
      //   borderColor: '#fff',
      //   data: [
      //     {
      //       name: '姓名1',
      //       src: 'static/img/mock/card/card-1.jpg',
      //       text: '介绍1'
      //     },
      //     {
      //       name: '姓名2',
      //       src: 'static/img/mock/card/card-2.jpg',
      //       text: '介绍2'
      //     },
      //     {
      //       name: '姓名3',
      //       src: 'static/img/mock/card/card-3.jpg',
      //       text: '介绍3'
      //     },
      //     {
      //       name: '姓名4',
      //       src: 'static/img/mock/card/card-4.jpg',
      //       text: '介绍4'
      //     }
      //   ]
      // },
      // easyDataOption1: {
      //   color: 'rgb(63, 161, 255)',
      //   span: 4,
      //   data: [
      //     {
      //       title: '今日注册',
      //       count: 0,
      //       icon: 'icon-cuowu'
      //     },
      //     {
      //       title: '今日登录',
      //       count: 22139,
      //       icon: 'icon-shujuzhanshi2'
      //     },
      //     {
      //       title: '今日订阅',
      //       count: 35623,
      //       icon: 'icon-jiaoseguanli'
      //     },
      //     {
      //       title: '今日评论',
      //       count: 16826,
      //       icon: 'icon-caidanguanli'
      //     },
      //     {
      //       title: '今日评论',
      //       count: 16826,
      //       icon: 'icon-caidanguanli'
      //     },
      //     {
      //       title: '今日评论',
      //       count: 16826,
      //       icon: 'icon-caidanguanli'
      //     }
      //   ]
      // },
      easyDataOption2: {
        color: 'rgb(63, 161, 255)',
        span: 4,
        discount: true,
        data: [
          {
            title: '没有回答的问题百分比%',
            count: 0,
            icon: 'icon-cuowu'
          },
          {
            title: '最热门问题收获回答',
            count: 0,
            icon: 'icon-shujuzhanshi2'
          },
          {
            title: '平均一个问题收获回答',
            count: 0.01,
            icon: 'icon-jiaoseguanli'
          },
          // {
          //   title: '菜单管理',
          //   count: 0,
          //   icon: 'icon-caidanguanli'
          // },
          // {
          //   title: '权限测试',
          //   count: 0,
          //   icon: 'icon-caidanguanli'
          // },
          // {
          //   title: '错误页面',
          //   count: 0,
          //   icon: 'icon-caidanguanli'
          // }
        ]
      }
    }
  },
  created() {},
  watch: {
    // 'easyDataOption2.data': {
    //   handler: function(newValue, oldValue) {
    //     console.log('data 变化了', newValue, oldValue);
    //     if (newValue[0] && newValue[0].count) {
    //       console.log('count 变化了', newValue[0].count);
    //     }
    //   },
    //   deep: true // 监听对象的变化
    // }
  },

  mounted() {
    this.fetchData() // 在组件挂载后调用 fetchData 方法获取实时数据
  },
  computed: {
    // formattedCount() {
    //   return this.easyDataOption2.data[2].count.toFixed(2);
    // }
  },

  methods: {
    fetchData() {
      // 发起 API 请求，获取最新的 count 数据
      // 使用 axios 或其他网络请求库
      axios.get('http://localhost:8080/questions/noAnswerQuestionCount').then(response => {
        // 更新 count 字段
        this.easyDataOption2.data[0].count = response.data*100/this.option.data[0].count
        this.easyDataOption.data[0].allcount=response.data*100/this.option.data[0].count
      }).catch(error => {
        console.error('Error:', error)
      })
      axios.get('http://localhost:8080/questions/maxAnswerCount').then(response => {
        this.easyDataOption2.data[1].count = response.data
        this.easyDataOption.data[0].count=response.data
      }).catch(error => {
        console.error('Error:', error)
      })
      axios.get('http://localhost:8080/questions/avgAnswerCount').then(response => {
        this.easyDataOption2.data[2].count = response.data
        this.easyDataOption.data[1].count=response.data
        this.easyDataOption.data[1].allcount=response.data
      }).catch(error => {
        console.error('Error:', error)
      })
      axios.get('http://localhost:8080/questions/mostActiveUsersPerQuestion').then(response => {
        this.easyDataOption.data[2].text = "最活跃用户: "+response.data.map(obj => ({
          theGuy: obj[5]
        }))[0].theGuy;
        this.easyDataOption.data[2].allcount = response.data.map(obj => ({
          theGuy: obj[6]
        }))[0].theGuy;
      }).catch(error => {
        console.error('Error:', error)
      })
      axios.get('http://localhost:8080/questions/countUniqueUsersPerQuestion').then(response => {
        this.easyDataOption.data[2].count = response.data[0][1];//!!!!!!!!!!!
        console.log(response.data);
        }
      ).catch(error => {
        console.error('Error:', error)
      })
    }
  }
}
</script>

<style scoped>
.item {
  margin-bottom: 16px;
}
</style>
<style lang ="scss">
    @import '../../styles/data-card.scss';
    @import '../../styles/data-display.scss';
    @import '../../styles/data-icons.scss';
    @import '../../styles/data-tabs.scss';
</style>
