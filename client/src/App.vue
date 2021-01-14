<template>
  <div>
    <Login v-if="!isLogin" v-on:onLoginSuccess="onLoginSuccess" :server="server"></Login>
    <Home v-if="isLogin" v-on:onLogout="onLogout" :server="server"></Home>
  </div>
</template>

<script>
import Home from "./views/Home";
import Login from "./views/Login";

export default {
  name: 'app',
  components: {
    Home,
    Login
  },
  data() {
    return {
      message: 'Hello world',
      isLogin: false,
      server: 'http://localhost:3000'
    };
  },
  methods: {
    onLoginSuccess() {
      console.log('Login Success!');
      this.isLogin = true
    },
    onLogout() {
      console.log('Logged Out');
      this.isLogin = false
    }
  },
  created() {
    this.isLogin = localStorage.access_token ? true : false
  }
};
</script>

<style scoped>
* {
  font-size: 14px;
}
</style>