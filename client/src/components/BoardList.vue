<template>
  <main class="board-list d-flex flex-column">
    <div class="d-flex justify-content-center">
        <!-- KANBAN -->
    </div>
    <TaskUpdate :task="task" :categories="categories" :server="server" :refreshTasks="refreshTasks" :toastMsg="toastMsg"></TaskUpdate>

    <div class="lists d-flex">
      <TaskBoard
        v-for="category in categories"
        :key="category.id"
        :id="category.id"
        :category="category"
        :tasks="tasks"
        :server="server"
        :refreshTasks='refreshTasks'
        :refreshCategories='refreshCategories'
        @updateClick="getDataTask"
        :toastMsg="toastMsg"
        :confirmDialog="confirmDialog">
      </TaskBoard>
      <div class="text-white align-self-start">
        <CategoryCreate :toastMsg="toastMsg" :server="server" :refreshCategories="refreshCategories"></CategoryCreate>
      </div>
    </div>
  </main>
</template>

<script>
import axios from 'axios'
import TaskBoard from "./Boards/TaskBoard";
import TaskUpdate from "./Boards/TaskUpdate";
import CategoryCreate from "./Boards/CategoryCreate"

export default {
  props: ['server', 'toastMsg', 'confirmDialog'],
  components: {
    TaskBoard,
    TaskUpdate,
    CategoryCreate
  },
  data() {
    return {
      tasks: [],
      // categories: [
      //   { id: 1, name: 'Backlog' },
      //   { id: 2, name: 'Todo' },
      //   { id: 3, name: 'Doing' },
      //   { id: 4, name: 'Done' }
      // ],
      categories: [],
      openModal: false,
      task: {}
    }
  },
  methods: {
    getDataTask(val) {
      this.task = val
    },
    getAllTasks() {
      axios({
        method: 'GET',
        url: this.server+'/tasks',
        headers : {
            access_token: localStorage.access_token
        }
      })
      .then((response) => {
        this.tasks = response.data
      })
      .catch((err) => {
        err.response.data.message.forEach(el => {
          this.toastMsg('error', el)
        });
      })
    },
    refreshTasks() {
      this.getAllTasks()
    },
    refreshCategories() {
      this.getAllCategories()
    },
    getAllCategories() {
      axios({
        method: 'GET',
        url: this.server+'/categories',
        headers : {
            access_token: localStorage.access_token
        }
      })
      .then((response) => {
        this.categories = response.data
      })
      .catch((err) => {
        err.response.data.message.forEach(el => {
          this.toastMsg('error', el)
        });
      })
    }
  },
  created() {
    this.getAllTasks()
    this.getAllCategories()
  }
}
</script>

<style>
  .board-list {
    overflow-y: auto; 
    height: 93vh;
  }
  .lists {
    flex-grow: 1; 
    overflow-x: auto; 
    overflow-y: hidden;
  }
</style>