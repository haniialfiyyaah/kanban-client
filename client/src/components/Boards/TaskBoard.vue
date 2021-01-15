<template>
  <div :id="id" class="task-board m-2 pb-4">
    <div class="task-board-content rounder-lg d-flex flex-column p-2">
      <div class="d-flex m-1">
        <strong class="h6">
          {{ category.name }}
        </strong>
      </div>
      <div :id="id" class="task-list pr-1 h-100">
        <draggable v-model="filterTasks" group="tasks" :sort="false" :move="checkMove" @end="onEnd" @start="drag=true" >
          <TaskItem
            v-for="task in filterTasks"
            :key="task.id"
            :id="task.id"
            :task="task"
            :server="server"
            :refreshTasks="refreshTasks"
            @updateClick="updateClick">
          </TaskItem>
        </draggable>
        <TaskCreate v-if="isInput" :closeInput="closeInput" :server="server" :category="category" :refreshTasks=refreshTasks></TaskCreate>
      </div>
      <button class="btn d-flex m-1 " v-if="!isInput" @click="openInput">
        + Add New Task
      </button>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable'
import TaskItem from "./TaskItem";
import TaskCreate from "./TaskCreate";
import axios from 'axios'

export default {
  props: ['id', 'category', 'tasks', 'server', 'refreshTasks'],
  components: {
    TaskItem,
    TaskCreate,
    draggable
  },
  data() {
    return {
      isForbiden: false,
      isInput: false,
      moveTaskId: 0,
      targetCategory: '',
      categoryTasks: this.filtered
    }
  },
  methods: {
    scrollToEnd: function() {    	
      const task_list = this.$el.querySelector(".task-list");
      task_list.scrollToEnd = task_list.scrollHeight;
    },
    openInput() {
      this.isInput = true
      this.scrollToEnd();
    },
    closeInput() {
      this.isInput = false
    },
    
    // Open modal update
    updateClick(val) {
      this.$emit('updateClick', val)
    },

    // Move Card
    changeCategory() {
      axios({
        method: 'PATCH',
        url: this.server+'/tasks/'+this.moveTaskId,
        headers : { access_token: localStorage.access_token },
        data: { category: this.targetCategory }
      })
      .then(({ data }) => {
        this.refreshTasks()
      })
      .catch((err) => {
        console.log(err);
      })
    },

    // Check if data not forbidden
    getOneTask(id) {
      console.log('called!');
      axios({
        method: 'GET',
        url: this.server+'/tasks/'+id,
        headers : { access_token: localStorage.access_token }
      })
      .then((response) => {
        this.isForbiden = true
      })
      .catch((err) => {
        this.isForbiden = false
      })
    },

    // stop or continue drag
    checkMove(evt) {
      const id = evt.draggedContext.element.id
      this.getOneTask(id)
      return this.isForbiden
    },

    // when card is drop!
    onEnd(e) {
      e.drop = false
      this.moveTaskId = +e.item.id
      this.targetCategory = e.to.parentElement.id
      this.changeCategory()
    }
  },
  computed: {
    filtered() {
      return this.tasks.filter(task => task.category.toLowerCase() === this.category.name.toLowerCase())
    },
    filterTasks: {
        get() {
          return !this.categoryTasks ? this.filtered : this.categoryTasks
        },
        set(value) {
          this.categoryTasks = value
          this.isForbiden = false
        }
    }
  }
}
</script>

<style>
  .task-board {
    min-width: 280px;
    height: 100%;
  }
  .task-board-content {
    background-color: #ebecf0; 
    max-height: 100%
  }
  .task-list {
    overflow-x: hidden;
    overflow-y: auto;
    z-index: 1;
    flex: 1 1 auto;
  }
</style>