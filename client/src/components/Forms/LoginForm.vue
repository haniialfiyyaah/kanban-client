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
      this.$emit('login', this.user)
    }
  }
}
</script>

<style>

</style>