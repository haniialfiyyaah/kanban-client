<template>
  <div :id="id" class="card bg-white shadow-sm mb-2">
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
  props: ['id', 'task', 'server', 'refreshTasks'],
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
      .then(({ data }) => {
        console.log(data);
        this.refreshTasks()
      })
      .catch((err) => {
        console.log(err.response.data.message);
      })
    }
  }
}
</script>

<style>
 .card {
   overflow:hidden;
   cursor: move;
 }
</style>