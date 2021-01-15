<template>
  <div>
    <form method="post" @submit.prevent="register">
      <div class="form-group">
        <label for="name">Name</label>
        <input v-model="user.name" type="text" class="form-control" id="name">
      </div>
      <div class="form-group">
        <label for="email">Email address</label>
        <input v-model="user.email" type="email" class="form-control" id="email" aria-describedby="emailHelp">
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input v-model="user.password" type="password" class="form-control" id="password">
      </div>
      <button type="submit" class="btn btn-primary">Register</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['server','toastMsg'],
  data() {
    return {
      user: {
        name: '',
        email: '',
        password: ''
      }
    }
  },
  methods: {
    register() {
      axios({
        method: 'POST',
        url: this.server+'/register',
        data : {
          name: this.user.name,
          email: this.user.email,
          password: this.user.password
        }
      })
      .then(({ data }) => {
        console.log(data);
        this.$emit('onRegisterSuccess', {
          email: this.user.email,
          password: this.user.password
        })
      })
      .catch(err => {
        console.log();
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