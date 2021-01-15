<template>
  <b-modal
    id="taskUpdate"
    ref="modal"
    title="Detail Task"
    okTitle="Edit"
    hide
    @show="resetModal"
    @hidden="resetModal"
    @ok="handleOk"
  >
    <form ref="form" @submit.prevent="updateTask">
      <b-form-group label="Name" label-for="name-input" invalid-feedback="Name is required">
        <b-form-input id="name-textarea" v-model="data.title" class="" required></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-3" label="Category:" label-for="input-3">
        <b-form-select
          id="category"
          v-model="data.category"
          :options="categoryName"
          required
        ></b-form-select>
      </b-form-group>
    </form>
  </b-modal>
</template>

<script>
import axios from 'axios'

export default {
  props: ['task', 'server', 'refreshTasks', 'categories'],
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
      .then(({ data }) => {
        console.log(data);
        this.refreshTasks()
      })
      .catch((err) => {
        console.log(err);
      })
    }
  },
  computed: {
    categoryName() {
      return this.categories.map(el => el.name)
    }
  }
}
</script>

<style>

</style>