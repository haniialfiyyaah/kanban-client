<template>
  <div>
    <form method="post" @submit.prevent="login">
      <div class="form-group">
        <label for="loginEmail">Email address</label>
        <input v-model="user.email" type="email" class="form-control" id="loginEmail" aria-describedby="emailHelp">
      </div>
      <div class="form-group">
        <label for="loginPassword">Password</label>
        <input v-model="user.password" type="password" class="form-control" id="loginPassword">
      </div>
      <button type="submit" class="btn btn-primary">Login</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  props: ['server'],
  data() {
    return {
      user: {
        email: '',
        password: ''
      }
    }
  },
  methods: {
    login() {
      axios({
        method: 'POST',
        url: this.server+'/login',
        data : {
            email: this.user.email,
            password: this.user.password
        }
      })
      .then(response => {
        console.log(response.data);
        localStorage.access_token = response.data.access_token
        this.$emit('onLoginSuccess', true)
        this.user.email = ''
        this.user.password = ''
      })
      .catch(err => {
        console.log(err.response.data.message);
      })
    }
  }
}
</script>

<style>

</style>