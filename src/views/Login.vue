<template>
  <div class="login">
    <el-form :model="loginForm" :rules="loginRules" ref="loginForm" label-width="100px" class="loginForm">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="loginForm.username"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" v-model="loginForm.password"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="resetForm('loginForm')">重置</el-button>
        <el-button type="primary" @click="submitForm('loginForm')">登陆</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      loginForm: {
        username: '',
        password: ''
      },
      loginRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          axios.post('/auth/login', this.form).then(res => {
            console.log(res)
            // this.$router.push('/about')
          }).catch(res => {
            console.log(res)
          }).finally(() => {
            this.loading = false
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm (formName) {
      this.$refs[formName].resetFields()
    },
    handleLogout () {
      axios.get('/auth/logout').then(res => {
        console.log(res)
      }).catch(res => {
        console.log(res)
      }).finally(() => {
      })
    },
    testLogin () {
      let self = this
      axios.get('/test').then(res => {
        self.testResponse = res
        console.log(res)
      }).catch(res => {
        self.testResponse = res
        console.log(res)
      })
    }
  }
}
</script>

<style scoped>

</style>
