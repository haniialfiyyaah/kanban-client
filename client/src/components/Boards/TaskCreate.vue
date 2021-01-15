<template>
  <form method="post" @submit.prevent="addTask">
    <div class="d-flex flex-column">
      <input ref="name" v-model="task.title" class="mb-2 p-2 pb-4" placeholder="Enter a Title">
      <div class="d-flex">
        <button class="btn btn-sm btn-primary" type="submit">Add New</button>
        <button class="btn btn-sm text-primary" @click="closeInput"> <strong>X</strong> </button>
      </div>
    </div>
  </form>
</template>

<script>
import axios from 'axios'

export default {
  props: ['closeInput', 'server', 'category', 'refreshTasks', 'toastMsg'],
  data() {
    return {
      task : {
        title: '',
        category: this.category.name
      }
    }
  },
  methods: {
    addTask() {
      axios({
        method: 'POST',
        url: this.server+'/tasks',
        headers : { access_token: localStorage.access_token },
        data: {
          title: this.task.title,
          category: this.task.category
        }
      })
      .then(({ data }) => {
        console.log(data);
        this.refreshTasks()
        this.task.title = ''
        this.closeInput()
        this.toastMsg('success', 'Task added')
      })
      .catch((err) => {
        err.response.data.message.forEach(el => {
          this.toastMsg('error', el)
        });
      })
    }
  },
  mounted() {
    this.$refs.name.focus();
  }
}
</script>

<style>

</style>