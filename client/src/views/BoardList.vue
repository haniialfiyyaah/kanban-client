<template>
  <main class="d-flex flex-column" style="overflow-y: auto; height: 92vh;">
      <div class="d-flex justify-content-center" style="height: auto;">
        <!-- KANBAN -->
        <!-- <b-button v-b-modal.modal-1>Launch demo modal</b-button> -->
        <TaskUpdate :task="task" :server="server" v-on:updateTasks="updateTasks"></TaskUpdate>
      </div>
      <div class="d-flex" style="flex-grow: 1; overflow-x: auto; overflow-y: hidden;">
        <TaskBoard :tasks="backlogGroup" :category="backlog" :server="server" v-on:updateTasks='updateTasks' v-on:updateClick="fetchUpdateData"></TaskBoard>
        <TaskBoard :tasks="todoGroup" :category="todo" :server="server" v-on:updateTasks='updateTasks' v-on:updateClick="fetchUpdateData"></TaskBoard>
        <TaskBoard :tasks="doingGroup" :category="doing" :server="server" v-on:updateTasks='updateTasks' v-on:updateClick="fetchUpdateData"></TaskBoard>
        <TaskBoard :tasks="doneGroup" :category="done" :server="server" v-on:updateTasks='updateTasks' v-on:updateClick="fetchUpdateData"></TaskBoard>
        <div class="text-white">@</div>
      </div>
      
  </main>
</template>

<script>
import axios from 'axios'
import TaskBoard from "../components/Boards/TaskBoard";
import TaskUpdate from "../components/Boards/TaskUpdate"

export default {
  props: ['server'],
  components: {
    TaskBoard,
    TaskUpdate
  },
  data() {
    return {
      tasks: [],
      category: '',
      openModal: false,
      task: {}
    }
  },
  methods: {
    fetchUpdateData(val) {
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
        console.log(err);
      })
    },
    updateTasks() {
      this.getAllTasks()
    }
  },
  created() {
    this.getAllTasks()
  },
  computed: {
    backlog() { return 'Backlog' },
    todo() { return 'Todo' },
    doing() { return 'Doing' },
    done() { return 'Done' },
    backlogGroup() {
      return this.tasks.filter(task => task.category.toLowerCase() === 'backlog')
    },
    todoGroup() {
      return this.tasks.filter(task => task.category.toLowerCase() === 'todo')
    },
    doingGroup() {
      return this.tasks.filter(task => task.category.toLowerCase() === 'doing')
    },
    doneGroup() {
      return this.tasks.filter(task => task.category.toLowerCase() === 'done')
    },
  }
}
</script>

<style>

</style>