<template>
  <div>
    <Login v-if="!isLogin" :onLoginSuccess="onLoginSuccess" :server="server"></Login>
    <Home v-if="isLogin" :onLogout="onLogout" :server="server"></Home>
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
      message: 'Welcome To Kanban',
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
      localStorage.clear()
      if (gapi.auth2) {
        const auth2 = gapi.auth2.getAuthInstance();
        auth2.signOut().then(function () {
          console.log('User signed out.');
        });
      }
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