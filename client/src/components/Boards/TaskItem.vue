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
  props: ['id', 'task', 'server', 'refreshTasks', 'confirmDialog', 'toastMsg'],
  methods: {
    updateClick() {
      this.$emit('updateClick', this.task)
    },
    deleteTask() {
      this.confirmDialog('Are you sure you wanna delete it?', 'This will be delete permanently', 'question', 'Yes, delete it!')
      .then(result => {
        if (result.isConfirmed) {
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
            this.toastMsg('success', data.message)
          })
          .catch((err) => {
            err.response.data.message.forEach(el => {
              this.toastMsg('error', el)
            });
            console.log(err.response.data.message);
          })
        }
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