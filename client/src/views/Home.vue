<template>
  <div>
    <Navbar :server="server" :logout='onLogout'></Navbar> 
    <BoardList :server="server" :toastMsg="toastMsg" :confirmDialog="confirmDialog"></BoardList>
  </div>
</template>

<script>
import Navbar from "../components/Navbar";
import BoardList from "../components/BoardList";

export default {
  props: ['server', 'onLogout', 'toastMsg', 'confirmDialog'],
  components: {
    Navbar,
    BoardList
  }
}
</script>

<style>

</style>