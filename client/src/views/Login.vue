<template>
  <div class="container">
    <br>
    <button class="btn btn-primary" @click="goTo('login')">LOGIN</button>
    <button class="btn btn-success" @click="goTo('register')">REGISTER</button>
    <LoginForm :server="server" class="mt-3" v-if="formPage === 'login'" v-on:onLoginSuccess="onLoginSuccess"></LoginForm>
    <RegisterForm :server="server" class="mt-3" v-if="formPage === 'register'" v-on:onRegisterSuccess="onRegisterSuccess"></RegisterForm>
  </div>
</template>

<script>
import LoginForm from "../components/Forms/LoginForm";
import RegisterForm from "../components/Forms/RegisterForm";

export default {
  props: ['server'],
  components: {
    LoginForm,
    RegisterForm
  },
  data() {
    return {
      formPage: 'login',
      // isLogin: false
    };
  },
  methods: {
    goTo(page) {
      this.formPage = page
    },
    onLoginSuccess() {
      // this.isLogin = val
      this.$emit('onLoginSuccess')
    },
    onRegisterSuccess() {
      console.log('Register Success! Please Login.');
      this.goTo('login')
    }
  },
  created() {
    
  }
}
</script>

<style>

</style>