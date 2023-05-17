<template>
	<section class="app-container">
		<!--工具条-->
<!--		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;">-->
<!--			<el-form :inline="true" :model="filters" @submit.native.prevent>-->
<!--				<el-form-item>-->
<!--					<el-input v-model="filters.questionId" placeholder="姓名"></el-input>-->
<!--				</el-form-item>-->
<!--				<el-form-item>-->
<!--					<el-button type="primary" v-on:click="getUsers">查询</el-button>-->
<!--				</el-form-item>-->
<!--				<el-form-item>-->
<!--					<el-button type="primary" @click="handleAdd">新增</el-button>-->
<!--				</el-form-item>-->
<!--			</el-form>-->
<!--		</el-col>-->

		<!--列表-->
		<el-table :data="users" highlight-current-row @selection-change="selsChange" style="width: 100%;">
			<el-table-column type="selection" width="55">
			</el-table-column>
			<el-table-column type="index" width="60">
			</el-table-column>
			<el-table-column prop="questionId" label="questionId" width="120">
      </el-table-column>
      <el-table-column prop="ownerAccountId" label="owner_accountId" width="120">
			</el-table-column>
      <el-table-column prop="isAnswered" label="is_answered" width="120">
        <template slot-scope="scope">
          {{ scope.row.isAnswered ? '是' : '否' }}
        </template>
      </el-table-column>
      <el-table-column prop="viewCount" label="view_count" width="120">
			</el-table-column>
			<el-table-column prop="protectedDate" label="protectedDate" width="120">
			</el-table-column>
			<el-table-column prop="acceptedAnswerId" label="accepted_answer_id" min-width="160">
			</el-table-column>
      <el-table-column prop="answerCount" label="answer_count" min-width="160">
      </el-table-column>
      <el-table-column prop="lastActivityDate" label="last_activity_date" min-width="160">
      </el-table-column>
      <el-table-column prop="creationDate" label="creation_date" min-width="160">
      </el-table-column>
      <el-table-column prop="lastEditDate" label="last_edit_date" min-width="160">
      </el-table-column>
      <el-table-column prop="contentLicense" label="content_license" min-width="160">
      </el-table-column>
      <el-table-column prop="link" label="link" min-width="160">
      </el-table-column>
      <el-table-column prop="title" label="title" min-width="160">
      </el-table-column>
<!--			<el-table-column label="操作" width="150">-->
<!--				<template slot-scope="scope">-->
<!--					<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>-->
<!--					<el-button type="danger" size="small" @click="handleDel(scope.$index, scope.row)">删除</el-button>-->
<!--				</template>-->
<!--			</el-table-column>-->
		</el-table>

<!--		&lt;!&ndash;工具条&ndash;&gt;-->
<!--		<el-col :span="24" class="toolbar">-->
<!--			<el-button type="danger" @click="batchRemove" :disabled="this.sels.length===0">批量删除</el-button>-->
<!--			<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="20" :total="total" style="float:right;">-->
<!--			</el-pagination>-->
<!--		</el-col>-->

<!--		&lt;!&ndash;编辑界面&ndash;&gt;-->
<!--		<el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible" :close-on-click-modal="false">-->
<!--			<el-form :model="editForm" label-width="80px" :rules="editFormRules" ref="editForm">-->
<!--				<el-form-item label="姓名" prop="questionId">-->
<!--					<el-input v-model="editForm.questionId" auto-complete="off"></el-input>-->
<!--				</el-form-item>-->
<!--				<el-form-item label="性别">-->
<!--					<el-radio-group v-model="editForm.sex">-->
<!--						<el-radio class="radio" :label=1>男</el-radio>-->
<!--						<el-radio class="radio" :label=0>女</el-radio>-->
<!--					</el-radio-group>-->
<!--				</el-form-item>-->
<!--				<el-form-item label="年龄">-->
<!--					<el-input-number v-model="editForm.age" :min="0" :max="200"></el-input-number>-->
<!--				</el-form-item>-->
<!--				<el-form-item label="生日">-->
<!--					<el-date-picker type="date" placeholder="选择日期" v-model="editForm.birth"></el-date-picker>-->
<!--				</el-form-item>-->
<!--				<el-form-item label="地址">-->
<!--					<el-input type="textarea" v-model="editForm.addr"></el-input>-->
<!--				</el-form-item>-->
<!--			</el-form>-->
<!--			<div slot="footer" class="dialog-footer">-->
<!--			 <el-button @click.native="dialogFormVisible=false">取消</el-button>-->
<!--			  <el-button v-if="dialogStatus=='create'" type="primary" @click="createData">添加</el-button>-->
<!--        <el-button v-else type="primary" @click="updateData">修改</el-button>-->
<!--			</div>-->
<!--		</el-dialog>-->
	</section>
</template>

<script>
import util from '@/utils/table.js'
import {
  getUserListPage,
  removeUser,
  batchRemoveUser,
  editUser,
  addUser
} from '@/api/userTable'
import axios from "axios";
import Vue from 'vue';
export default {
  data() {
    return {
      dialogStatus: '',
      textMap: {
        update: 'Edit',
        create: 'Create'
      },
      dialogFormVisible: false,
      filters: {
        questionId: ''
      },
      users: [],
      total: 0,
      page: 1,
      sels: [], // 列表选中列
      editFormRules: {
        questionId: [{ required: true, message: '请输入姓名', trigger: 'blur' }]
      },
      // 编辑界面数据
      editForm: {
        id: '0',
        questionId: '',
        sex: 1,
        age: 0,
        birth: '',
        addr: ''
      },

      addFormVisible: false, // 新增界面是否显示
      addFormRules: {
        questionId: [{ required: true, message: '请输入姓名', trigger: 'blur' }]
      }
    }
  },
  methods: {
    // // 性别显示转换
    // formatSex: function(row, column) {
    //   return row.sex === 1 ? '男' : row.sex === 0 ? '女' : '未知'
    // },
    // handleCurrentChange(val) {
    //   this.page = val
    //   this.getUsers()
    // },
    // // 获取用户列表
    // getUsers() {
    //
    //   const para = {
    //     page: this.page,
    //     name: this.filters.name
    //   }
    //   getUserListPage(para).then(res => {
    //     this.total = res.data.total
    //     this.users = res.data.users
    //   })
    // },
    // 获取用户列表
    getUsers() {

        // axios.get('http://localhost:8080/questions').then(response => {
        //   this.users = response.data;
        //   console.log('userV页面获取的数据为' + this.users.questionId);
        //   console.log('userV页面获取的数据为' + this.users.is_answered);
        //   console.log('userV页面获取的数据为' + this.users.view_count);
        //   console.log('获取到的问题数量为' + this.users.length);
        // });


      axios.get('http://localhost:8080/questions' ).then(function (resp) {
        this.users = resp.data;
        console.log('获取到的问题数量为' + this.users.length);
        this.users.forEach(function (question) {
          console.log('Question ID: ' + question.questionId);
          console.log('Is Answered: ' + question.ownerAccountId);
          console.log('View Count: ' + question.viewCount);
        });
        Vue.set(this, 'users', this.users);
      }.bind(this));

        // const self = this;
        // axios.get('http://localhost:8080/questions').then(function (resp) {
        //   self.users = resp.data;
        //   console.log('userV页面获取的数据为' + self.users.questionId);
        //   console.log('userV页面获取的数据为' + self.users.is_answered);
        //   console.log('userV页面获取的数据为' + self.users.view_count);
        //   console.log('获取到的问题数量为' + self.users.length);
        // });



    },
    // 删除
    handleDel(index, row) {
      this.$confirm('确认删除该记录吗?', '提示', {
        type: 'warning'
      })
        .then(() => {
          const para = { id: row.id }
          removeUser(para).then(res => {
            this.$message({
              message: '删除成功',
              type: 'success'
            })
            this.getUsers()
          })
        })
        .catch(() => {})
    },
    // 显示编辑界面
    handleEdit(index, row) {
      this.dialogStatus = 'update'
      this.dialogFormVisible = true
      this.editForm = Object.assign({}, row)
    },
    // 显示新增界面
    handleAdd() {
      this.dialogStatus = 'create'
      this.dialogFormVisible = true
      this.editForm = {
        id: '0',
        questionId: '',
        sex: 1,
        age: 0,
        birth: '',
        addr: ''
      }
    },
    // 编辑
    updateData() {
      this.$refs.editForm.validate(valid => {
        if (valid) {
          this.$confirm('确认提交吗？', '提示', {})
            .then(() => {
              const para = Object.assign({}, this.editForm)
              para.birth =
                !para.birth || para.birth === ''
                  ? ''
                  : util.formatDate.format(new Date(para.birth), 'yyyy-MM-dd')
              editUser(para).then(res => {
                this.$message({
                  message: '提交成功',
                  type: 'success'
                })
                this.$refs['editForm'].resetFields()
                this.dialogFormVisible = false
                this.getUsers()
              })
            })
            .catch(e => {
              // 打印一下错误
              console.log(e)
            })
        }
      })
    },
    // 新增
    createData: function() {
      this.$refs.editForm.validate(valid => {
        if (valid) {
          this.$confirm('确认提交吗？', '提示', {})
            .then(() => {
              this.editForm.id = (parseInt(Math.random() * 100)).toString() // mock a id
              const para = Object.assign({}, this.editForm)
              console.log(para)

              para.birth =
                !para.birth || para.birth === ''
                  ? ''
                  : util.formatDate.format(new Date(para.birth), 'yyyy-MM-dd')
              addUser(para).then(res => {
                this.$message({
                  message: '提交成功',
                  type: 'success'
                })
                this.$refs['editForm'].resetFields()
                this.dialogFormVisible = false
                this.getUsers()
              })
            })
            .catch(e => {
              // 打印一下错误
              console.log(e)
            })
        }
      })
    },
    // 全选单选多选
    selsChange(sels) {
      this.sels = sels
    },
    // 批量删除
    batchRemove() {
      var ids = this.sels.map(item => item.id).toString()
      this.$confirm('确认删除选中记录吗？', '提示', {
        type: 'warning'
      })
        .then(() => {
          const para = { ids: ids }
          batchRemoveUser(para).then(res => {
            this.$message({
              message: '删除成功',
              type: 'success'
            })
            this.getUsers()
          })
        })
        .catch(() => {})
    }
  },
  mounted() {
    this.getUsers()
  }
}
</script>

<style scoped>

</style>
