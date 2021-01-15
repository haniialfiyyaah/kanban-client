<template>
  <nav class="bg-light">
    <div class="mr-3 ml-3 d-flex justify-content-between align-items-center">
      <a class="navbar-brand " href="">
        <img src="https://mustopa28121992.github.io/projectt/img/fox--md5--a9e377ae39495073d0e66db163fc8d9b.png" width="30" height="30" class="d-inline-block align-top" alt="" loading="lazy">
      </a>
      <strong> Kanban Board </strong>
      <div class="d-flex">
        <!-- <button class="btn btn-success btn-sm mr-3" >Add Category</button> -->
        <button class="btn btn-warning btn-sm" @click="logout" >Logout</button>
        <!-- <LoginGoogle @click="logout" :isLogin="false"></LoginGoogle> -->
      </div>
    </div>
  </nav>
</template>

<script>
import LoginGoogle from './Forms/LoginGoogle'

export default {
  props: ['logout'],
  components: {
    LoginGoogle
  }
}
</script>

<style>
  nav {
    min-height: 40px;
    max-height: 40px;
  }
</style>