<template>
  <b-modal
    id="taskUpdate"
    ref="modal"
    title="Update Task"
    okTitle="Edit"
    hide
    @show="resetModal"
    @hidden="resetModal"
    @ok="handleOk"
  >
    <form ref="form" @submit.prevent="updateTask">
      <b-form-group label="Name" label-for="name-input" invalid-feedback="Name is required">
        <b-form-textarea id="name-textarea" v-model="data.title" required></b-form-textarea>
      </b-form-group>
    </form>
  </b-modal>
</template>

<script>
import axios from 'axios'

export default {
  props: ['task', 'server'],
  data() {
    return {
      data: {
        id: this.task.id,
        title: this.task.title,
        category: this.task.category,
        UserId: this.task.UserId
      }
    }
  },
  methods: {
    resetModal() {
      this.data.id = this.task.id
      this.data.title = this.task.title
      this.data.category = this.task.category
      this.data.UserId = this.task.UserId
    },
    handleOk() {
      this.updateTask()
    },
    updateTask() {
      axios({
        method: 'PUT',
        url: this.server+'/tasks/'+this.task.id,
        headers : {
            access_token: localStorage.access_token
        },
        data: {
          title: this.data.title,
          category: this.data.category
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
  },
  created() {
    console.log('Modal Created');
  }
}
</script>

<style>

</style>