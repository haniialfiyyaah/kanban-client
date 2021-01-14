<template>
  <div class="card bg-white shadow-sm mb-2" style="overflow:hidden;">
    <div class="card-body p-2 d-flex flex-column">
      {{ task.title }}
      <div class="align-self-end">
        <b-button class="btn btn-sm bg-primary" v-b-modal.taskUpdate @click="updateClick">U</b-button>
        <b-button class="btn btn-sm bg-danger" @click="deleteTask">D</b-button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  props: ['task', 'server'],
  methods: {
    updateClick() {
      this.$emit('updateClick', this.task)
    },
    deleteTask() {
      axios({
        method: 'DELETE',
        url: this.server+'/tasks/'+this.task.id,
        headers : {
            access_token: localStorage.access_token
        }
      })
      .then((response) => {
        console.log(response.data);
        this.$emit('updateTasks')
      })
      .catch((err) => {
        console.log(err);
      })
    }
  }
}
</script>

<style>

</style>