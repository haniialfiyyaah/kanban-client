<template>
  <div>
    <Login v-if="!isLogin" :onLoginSuccess="onLoginSuccess" :server="server"></Login>
    <Home v-if="isLogin" :onLogout="onLogout" :server="server" :toastMsg="toastMsg" :confirmDialog="confirmDialog"></Home>
  </div>
</template>

<script>
import Home from "./views/Home";
import Login from "./views/Login";
import Swal from 'sweetalert2'

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
      server: 'https://protected-sierra-94699.herokuapp.com'
      // server: 'http://localhost:3000'
      
    };
  },
  methods: {
    onLoginSuccess() {
      console.log('Login Success!');
      this.isLogin = true
      this.successAlert('You are now login!')
    },
    onLogout() {
      this.confirmDialog('You wanna go?', 'You can come back later', 'info', 'Logout')
        .then(result => {
          localStorage.clear()
          if (gapi.auth2) {
            const auth2 = gapi.auth2.getAuthInstance();
            auth2.signOut().then(function () {
              console.log('User signed out.');
            });
          }
          this.isLogin = false
          this.alertMsg('Logout', 'You are logged out', 'success')
        })
    },
    showAddCategory() {
    //   const { value: ipAddress } = await Swal.fire({
    //   title: 'Enter your IP address',
    //   input: 'text',
    //   inputLabel: 'Your IP address',
    //   inputValue: inputValue,
    //   showCancelButton: true,
    //   inputValidator: (value) => {
    //     if (!value) {
    //       return 'You need to write something!'
    //     }
    //   }
    // })

    // if (ipAddress) {
    //   Swal.fire(`Your IP address is ${ipAddress}`)
    // }
    },

    // Alert
    alertMsg(title, text, icon) {
      return Swal.fire( title, text, icon )
    },
    successAlert(message) {
      return Swal.fire({
        position: 'center',
        icon: 'success',
        title: message,
        showConfirmButton: false,
        timer: 1500
      })
    },
    confirmDialog(title, text, icon, confirmButtonText) {
      return Swal.fire({
        title: title,
        text: text,
        icon: icon,
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: confirmButtonText
      })
    },
    toastMsg(icon, title) {
      const Toast = Swal.mixin({
        toast: true,
        position: 'bottom-end',
        showConfirmButton: false,
        timer: 3000,
        timerProgressBar: true,
        didOpen: (toast) => {
            toast.addEventListener('mouseenter', Swal.stopTimer)
            toast.addEventListener('mouseleave', Swal.resumeTimer)
          }
      })
      Toast.fire({icon,title})
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