<template>
  <div class="app-container">
    <el-form ref="form" :model="form" label-width="120px">
      <el-row>
        <el-col :span="8" class="grid-content">
          <el-form-item label="用户名：">
            <el-input v-model="form.name" />
          </el-form-item>
        </el-col>
        <el-col :span="8" class="grid-content">
          <el-form-item label="显示名：">
            <el-input v-model="form.nickname" />
          </el-form-item>
        </el-col>
      </el-row>
      <el-form-item label="密码：">
        <el-input v-model="form.nickname" class="pwd" />
      </el-form-item>
      <el-form-item label="再输入密码：">
        <el-input v-model="form.nickname" class="pwd" />
      </el-form-item>
      <el-form-item label="性别：">
        <el-radio v-model="form.gender" label="1">男</el-radio>
        <el-radio v-model="form.gender" label="0">女</el-radio>
      </el-form-item>
      <el-form-item label="出生年月">
        <el-col :span="11">
          <el-date-picker
            v-model="form.date"
            type="date"
            placeholder="Pick a date"
            style="width: 100%;"
          />
        </el-col>
      </el-form-item>
      <el-form-item label="Instant delivery">
        <el-switch v-model="form.delivery" />
      </el-form-item>
      <el-form-item label="用户类型：">
        <el-checkbox-group v-model="form.type">
          <el-checkbox label="超级管理员" name="type" />
          <el-checkbox label="商城管理员" name="type" />
          <el-checkbox label="普通用户" name="type" />
        </el-checkbox-group>
      </el-form-item>
      <el-form-item label="Resources">
        <el-radio-group v-model="form.resource">
          <el-radio label="Sponsor" />
          <el-radio label="Venue" />
        </el-radio-group>
      </el-form-item>
      <el-form-item label="Activity form">
        <el-input v-model="form.desc" type="textarea" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">Create</el-button>
        <el-button @click="onCancel">Cancel</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import { addUser } from '@/api/user'
export default {
  name: 'AddUser',
  data() {
    return {
      form: {
        userName: '',
        passWord: '',
        region: '',
        gender: '1',
        date: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      }
    }
  },
  methods: {
    onSubmit() {
      this.$message('submit!')
      this.loading = true
      addUser(this.form)
        .then(response => {
          console.log(response)
          // this.loading = false
          // this.$router.push({ path: this.redirect || '/' })
        })
        .catch(response => {
          //   this.$notify.error({
          //     title: "失败",
          //     message: response.data.errmsg
          //   });
          console.log(response)
          this.loading = false
        })
    },
    onCancel() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    }
  }
}
</script>

<style scoped>
.line {
  text-align: center;
}
.pwd {
  width: 284px;
}
</style>
