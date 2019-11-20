<template>
  <div class="login">
    <el-row style="z-index: 1;height: 100%;">
      <el-card class="login-box"  element-loading-background="rgba(0, 0, 0, 0.8)">
        <el-form ref="form" :model="form" :rules="rules" label-with="80px" @keyup.enter.native="handleSubmit">
          <h1 class="title">创造传奇</h1>
          <el-form-item prop="username">
            <el-input v-model="form.username" :autofocus="true" placeholder="请输入用户">
              <template slot="prepend">
                <i class="fa fa-user"></i>
              </template>
            </el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input type="password" v-model="form.password" placeholder="请输入密码" >
              <template slot="prepend">
                <i class="fa fa-lock"></i>
              </template>
            </el-input>
          </el-form-item>
          <el-form-item>
            <el-alert v-if="error" :title="error" type="error" style="margin-top: -10px; margin-bottom: 10px" show-icon></el-alert>
            <el-button type="primary" :loading="loading" @click="handleSubmit" style="width: 100%">登录</el-button>
          </el-form-item>
        </el-form>
      </el-card>
      <el-button type="primary" @click="testLogin">测试</el-button>
      <el-button type="primary" @click="handleLogout">注销</el-button>
      <div>{{testResponse}}</div>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Login',
  data () {
    return {
      loading: false,
      error: '',
      form: {
        username: '',
        password: ''
      },
      testResponse: '',
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    handleSubmit () {
      this.error = ''
      this.$refs['form'].validate(pass => {
        if (!pass) {
          return false
        }
        this.loading = true
        axios.post('/auth/login', this.form).then(res => {
          console.log(res)
          // this.$router.push('/about')
        }).catch(res => {
          console.log(res)
        }).finally(() => {
          this.loading = false
        })
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
  },
  watch: {
    form: {
      handler: function () {
        this.error = ''
      },
      deep: true
    }
  }
}
</script>

<style scoped>
  .login {
    background: white;
    background-size: 100% 100%;
    width: 100%;
    height: 100%;
    position: fixed;
  }
  .login-box {
    background: rgba(0, 0, 0, 0.5);
    border: none;
    width: 25%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate3d(-50%, -50%,0);
    -webkit-transform: translate3d(-50%, -50%,0);
  }
  .login-box-msg {
    color: #ffffff;
    text-align: center;
  }
  .login-box .title {
    color: #ffffff;
    text-align: center;
  }
</style>
