<template>
  <div :id="id" class="task-board m-2 pb-4">
    <div class="task-board-content rounder-lg d-flex flex-column p-2">
      <!-- CATEGORY UPDATE -->
      <form action="" @submit.prevent="updateCategory">
        <div class="d-flex flex-column align-items-center m-1" v-if="isUpdate">
            <input ref="name" v-model="category.name" class="mb-2 p-2 pb-4 w-100" placeholder="Enter Category Name">
            <div class="align-self-end">
              <button type="submit" class="btn btn-sm btn-success" @click="updateCategory">UPDATE</button>
              <button class="btn btn-sm btn-secondary" @click="closeUpdate">CANCEL</button>
            </div>
          <hr>
        </div>
      </form>
      <!-- CATEGORY NAME -->
      <div class="d-flex justify-content-between align-items-center m-1"  v-if="!isUpdate">
        <strong class="h6">
          {{ category.name }}
        </strong>
        <div>
          <button class="btn btn-sm btn-outline-secondary" @click="openUpdate">U</button>
          <button class="btn btn-sm btn-outline-danger" @click="deleteCategory">D</button>
        </div>
      </div>
      <div :id="id" class="task-list pr-1 h-100">
        <draggable :list="filtered" group="tasks" :sort="false" :move="checkMove" @end="drag=false" @start="drag=true" >
          <TaskItem
            v-for="task in filtered"
            :key="task.id"
            :id="task.id"
            :task="task"
            :server="server"
            :refreshTasks="refreshTasks"
            @updateClick="updateClick"
            :toastMsg="toastMsg"
            :confirmDialog="confirmDialog">
          </TaskItem>
        </draggable>
        <TaskCreate v-if="isInput" :toastMsg="toastMsg" :closeInput="closeInput" :server="server" :category="category" :refreshTasks=refreshTasks></TaskCreate>
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
  props: ['id', 'category', 'tasks', 'server', 'refreshTasks', 'toastMsg', 'confirmDialog', 'refreshCategories'],
  components: {
    TaskItem,
    TaskCreate,
    draggable
  },
  data() {
    return {
      isForbiden: true,
      isInput: false,
      isUpdate: false,
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
    openUpdate() {
      this.isUpdate = true
    },
    closeUpdate() {
      this.isUpdate = false
    },

    deleteCategory() {
      this.confirmDialog('Are you sure you wanna delete it?', 'Tasks inside this category will also deleted!', 'question', 'Yes, delete it!')
      .then(result => {
        if (result.isConfirmed) {
          axios({
            method: 'DELETE',
            url: this.server+'/categories/'+this.category.id,
            headers : {
                access_token: localStorage.access_token
            }
          })
          .then(({ data }) => {
            console.log(data);
            this.refreshCategories()
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
    },

    updateCategory() {
      axios({
        method: 'PUT',
        url: this.server+'/categories/'+this.category.id,
        headers : {
            access_token: localStorage.access_token
        },
        data: {
          name: this.category.name
        }
      })
      .then(({ data }) => {
        console.log(data);
        this.refreshCategories()
        this.closeUpdate()
        this.toastMsg('success', 'Category Updated')
      })
      .catch((err) => {
        err.response.data.message.forEach(el => {
          this.toastMsg('error', el)
        });
        console.log(err.response.data.message);
      })
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
        data: { CategoryId: this.targetCategory }
      })
      .then(({ data }) => {
        this.refreshTasks()
        this.isForbiden = true
        this.toastMsg('success','tasks moved')
      })
      .catch((err) => {
        err.response.data.message.forEach(el => {
          this.toastMsg('error', el)
        });
        this.isForbiden = false
      })
    },

    // stop or continue drag
    checkMove(evt) {
      this.moveTaskId = evt.draggedContext.element.id
      this.targetCategory = evt.to.parentElement.id
      this.changeCategory()
      return this.isForbiden
    }

  },
  computed: {
    filtered() {
      return this.tasks.filter(task => task.CategoryId === this.category.id)
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