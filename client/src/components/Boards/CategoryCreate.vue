<template>
<div>
<div class="task-board m-2 pb-4">
  <div class="task-board-content rounder-lg d-flex flex-column p-2">
    <div class="d-flex m-1">
      <strong class="h6 text-dark">
        Add New Category
      </strong>
    </div>
    <div class="task-list pr-1 h-100">
      <form method="post" @submit.prevent="addCategory" v-if="isInput">
        <div class="d-flex flex-column">
          <input ref="name" v-model="category" class="mb-2 p-2 pb-4" placeholder="Enter Category Name">
          <div class="d-flex">
            <button class="btn btn-sm btn-primary" type="submit">Add Category</button>
            <button class="btn btn-sm text-primary" @click="closeInput"> <strong>X</strong> </button>
          </div>
        </div>
      </form>
    </div>
    <button class="btn bg-success text-white mb-2" v-if="!isInput" @click="openInput">+</button>
  </div>
</div>



  
</div>
</template>

<script>
import axios from 'axios'

export default {
  props: ['server', 'refreshCategories', 'toastMsg'],
  data() {
    return {
      category: '',
      isInput: false
    }
  },
  methods: {
    openInput() {
      this.isInput = true
      // this.$refs.name.focus();
    },
    closeInput() {
      this.isInput = false
    },
    
    addCategory() {
      axios({
        method: 'POST',
        url: this.server+'/categories',
        headers : { access_token: localStorage.access_token },
        data: {
          name: this.category
        }
      })
      .then(({ data }) => {
        console.log(data);
        this.refreshCategories()
        this.category = ''
        this.closeInput()
        this.toastMsg('success', 'New Category added')
      })
      .catch((err) => {
        console.log(err);
        err.response.data.message.forEach(el => {
          this.toastMsg('error', el)
        });
      })
    }
  },
}
</script>

<style>

</style>