<template>
  <div class="container">
    <br>
    <button class="btn btn-primary" @click="goTo('login')">LOGIN</button>
    <button class="btn btn-success" @click="goTo('register')">REGISTER</button>
    <LoginForm :server="server" class="mt-3" v-if="formPage === 'login'" @login="login"></LoginForm>
    <RegisterForm :server="server" class="mt-3" v-if="formPage === 'register'" :toastMsg="toastMsg" @onRegisterSuccess="onRegisterSuccess"></RegisterForm>
    <span>-- or --</span>
    <LoginGoogle :server="server" :onLoginSuccess="onLoginSuccess" :isLogin="true"></LoginGoogle>
  </div>
</template>

<script>
import axios from 'axios';
import LoginForm from "../components/Forms/LoginForm";
import RegisterForm from "../components/Forms/RegisterForm";
import LoginGoogle from "../components/Forms/LoginGoogle";

export default {
  props: ['server', 'onLoginSuccess', 'toastMsg'],
  components: {
    LoginGoogle,
    LoginForm,
    RegisterForm
  },
  data() {
    return {
      formPage: 'login'
    };
  },
  methods: {
    goTo(page) {
      this.formPage = page
    },
    onRegisterSuccess(user) {
      console.log('Register Success! Loging in.');
      this.login(user)
    },
    login(user) {
      axios({
        method: 'POST',
        url: this.server+'/login',
        data : user
      })
      .then(({ data }) => {
        localStorage.access_token = data.access_token
        this.onLoginSuccess()
      })
      .catch(err => {
        console.log(err);
        err.response.data.message.forEach(el => {
          this.toastMsg('error', el)
        });
      })
    }
  }
}
</script>

<style>

</style>