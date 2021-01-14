<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">
          <img src="https://mustopa28121992.github.io/projectt/img/fox--md5--a9e377ae39495073d0e66db163fc8d9b.png" width="30" height="30" class="d-inline-block align-top" alt="" loading="lazy">
          Hacktiv8    
      </a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Tasks</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
        </ul>
        <div class="pe-2">
          <button class="btn" type="button">Login</button>
          <button class="btn btn-outline-success" type="button">Register</button>
        </div>
        <div>
          <button class="btn btn-success" type="button">Add Task</button>
          <button class="btn btn-warning" type="button" @click="logout" >Logout</button>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  methods: {
    logout() {
      this.$emit('logout')
    }
  }
}
</script>

<style>

</style>