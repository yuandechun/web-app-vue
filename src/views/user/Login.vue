<template>

  <body id="poster">
    <el-form :model="loginForm"
             class="login-container"
             label-position="left"
             label-width="40px">
      <h3 class="login_title">系统登陆</h3>
      <el-form-item label="用户">
        <el-input type="text"
                  prefix-icon="el-icon-user"
                  v-model="loginForm.userName"
                  auto-complete="off"
                  placeholder="账号"></el-input>
      </el-form-item>

      <el-form-item label="密码">
        <el-input type="password"
                  prefix-icon="el-icon-key"
                  v-model="loginForm.password"
                  auto-complete="off"
                  placeholder="密码"></el-input>
      </el-form-item>

      <el-form-item style="width: 100%">
        <el-button type="primary"
                   class="login-button-width"
                   v-on:click="login">登陆账号</el-button>
        <el-button type="text">
          <router-link to="/register">注册账号</router-link>
        </el-button>
      </el-form-item>
    </el-form>
  </body>
</template>
 
 
<script>
import { mapMutations } from 'vuex';

export default {
  name: "Login",
  data () {
    return {
      messages: [],
      loginForm: {
        userName: '',
        password: ''
      },
    }
  },

  methods: {
    ...mapMutations(['setToken']),
    // 登陆
    login () {
      this.$post(this.$api.login, this.loginForm)
        .then(res => {
          if (res.status == 'SUCCESS') {
            //sessionStorage
            sessionStorage.setItem('username', res.data.userName)
            //store token
            this.setToken({ Authorization: res.data.token })
            this.$router.push({ path: '/home', querry: { redirect: this.$router.currentRoute.fullPath } })
          } else {
            this.$router.push({ path: '/page404' })
          }
        })
    }
  }


}
</script>
 
<style>
#poster {
  background: url("./../../assets/eva.jpg") no-repeat;
  background-position: center;
  height: 100%;
  width: 100%;
  background-size: cover;
  position: fixed;
}
body {
  margin: 0px;
  padding: 0;
}

.login-container {
  border-radius: 15px;
  background-clip: padding-box;
  margin: 120px auto;
  width: 25%;
  padding: 35px 35px 15px 35px;
  background: #fff;
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
}

.login_title {
  margin: 0px auto 40px auto;
  text-align: center;
  color: #505458;
}

.login-button-width {
  width: 100%;
}
</style>