<template>
  <div class="m-2 pb-4" style="min-width: 280px; height: 100%;">
    <div class="rounder-lg d-flex flex-column p-2" style="background-color: #ebecf0; max-height: 100%">
      <div class="d-flex m-1">
        <h6>{{ category }}</h6>
        <!-- {{ tasks[0].title }} -->
        <!-- <button class="btn btn-sm align-self-end">Edit</button> -->
      </div>
      <div id="container" class="pr-1 h-100"  style="overflow-x: hidden; overflow-y: auto; z-index: 1; flex: 1 1 auto;">
        <TaskItem v-for="task in tasks" :key="task.id" :task="task" :server="server" v-on:updateClick="updateClick" v-on:updateTasks='updateTasks'></TaskItem>
        <form method="post" v-if="isInput" @submit.prevent="addTask">
          <div class="d-flex flex-column">
            <textarea v-model="task.title" type="text" class="mb-2 p-1" placeholder="Enter a Title"></textarea>
            <div class="d-flex">
              <button class="btn btn-sm btn-primary" type="submit">Add New</button>
              <button class="btn btn-sm text-primary" @click="closeInput"> <strong>X</strong> </button>
            </div>
          </div>
        </form>
      </div>
      <button class="btn d-flex m-1" v-if="!isInput" @click="openInput">
        + Add New Task
      </button>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import TaskItem from "./TaskItem";

export default {
  props: ['tasks', 'category', 'server'],
  components: {
    TaskItem
  },
  data() {
    return {
      isInput: false,
      task : {
        title: '',
        category: this.category
      }
    }
  },
  methods: {
    scrollToEnd: function() {    	
      const container = this.$el.querySelector("#container");
      container.scrollTop = container.scrollHeight;
    },
    openInput() {
      this.isInput = true
      this.scrollToEnd();
    },
    closeInput() {
      this.isInput = false
    },
    updateClick(val) {
      this.$emit('updateClick', val)
    },
    updateTasks() {
      this.$emit('updateTasks')
    },
    addTask() {
      axios({
        method: 'POST',
        url: this.server+'/tasks',
        headers : {
            access_token: localStorage.access_token
        },
        data: {
          title: this.task.title,
          category: this.task.category
        }
      })
      .then((response) => {
        console.log(response.data);
        this.$emit('updateTasks')
        this.task.title = ''
        this.closeInput()
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