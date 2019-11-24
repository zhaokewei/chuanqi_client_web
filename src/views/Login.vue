<template>
  <div class="login">
    <el-row class="login-box">
      <el-form :model="loginForm" :rules="loginRules" ref="loginForm" label-width="100px" class="loginForm">
        <el-form-item label="用户名" prop="username">
          <el-input v-model="loginForm.username"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="password" v-model="loginForm.password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-alert v-if="errorInfo" :title="errorInfo" type="error" show-icon></el-alert>
          <el-button type="primary" :loading="loading" style="width: 100%" @click="submitForm('loginForm')">登陆</el-button>
        </el-form-item>
      </el-form>
    </el-row>
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
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      },
      loading: false,
      errorInfo: ''
    }
  },
  methods: {
    submitForm (formName) {
      let self = this
      this.$refs[formName].validate((valid) => {
        if (valid) {
          self.loading = true
          axios.post('/auth/login', this.loginForm).then(res => {
            if (res.data.code === 0) {
              this.$router.push('/home')
              return true
            } else {
              this.errorInfo = res.data.msg
              return false
            }
          }).catch(res => {
            this.errorInfo = 'network connect fail'
          }).finally(() => {
            self.loading = false
          })
        } else {
          return false
        }
      })
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
  .login {
    width: 100%;
    height: 100%;
  }
.login-box {
  width: 25%;
  position: absolute;
  left: 50%;
  top: 50%;
}
</style>
