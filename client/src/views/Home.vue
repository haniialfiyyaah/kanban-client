<template>
<!-- justify-content-center align-items-stretch -->
  <div>
    <!-- navbar min height max height 40px -->
    <!-- <button @click="logout">LOGOUT</button> -->
    <Navbar :server="server" v-on:logout='logout'></Navbar> 
    <BoardList :server="server"></BoardList>

  </div>
</template>

<script>
import Navbar from "../components/Navbar";
import BoardList from "./BoardList";

export default {
  props: ['server'],
  components: {
    Navbar,
    BoardList
  },
  methods: {
    logout() {
      localStorage.clear()
      this.$emit('onLogout')
    }
  }
}
</script>

<style>

</style>