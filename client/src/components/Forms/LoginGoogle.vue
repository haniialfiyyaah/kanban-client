<template>
  <div>
    <GoogleLogin v-if="isLogin" :params="params" :renderParams="renderParams" :onSuccess="onSignIn" :onFailure="onFailure">Login</GoogleLogin>
    <GoogleLogin v-if="!isLogin" class="btn btn-sm bg-danger text-white" :params="params" :logoutButton=true>Logout</GoogleLogin>  
  </div>
  
</template>

<script>
import axios from 'axios'
import GoogleLogin from 'vue-google-login'

export default {
  props: ['server', 'onLoginSuccess', 'isLogin'],
  components: {
    GoogleLogin
  },
  data() {
    return {
      params: {
        client_id: "853610253874-iqjeb7nc1vtnc3mvqiq1hsn10hhf6iqe.apps.googleusercontent.com"
      },
      renderParams: {
        'scope': 'profile email',
        'width': 240,
        'height': 50,
        'longtitle': true,
        'theme': 'dark',
      }
    }
  },
  methods: {
    onFailure(err) {
      console.log(err);
    },
    onSignIn(googleUser) {
      const profile = googleUser.getBasicProfile();
      const id_token = googleUser.getAuthResponse().id_token;
      axios({
        method: 'POST',
        url: this.server+'/loginGoogle',
        data : {
          id_token
        }
      })
      .then(({ data }) => {
        localStorage.access_token = data.access_token
        this.onLoginSuccess()
      })
      .catch(err => {
        console.log(err);
      })
    }
  }
}
</script>

<style>

</style>