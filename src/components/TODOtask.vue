<template>
  <input v-model="newTask" type="text" placeholder="ENTER YOUR TASK HERE..." />
  <button class="add-btn" @click="addTask">ADD</button>

  <!-- <div class="filters">
    <label>
      <input v-model="selectedFilter" value="completed" type="checkbox" />COMPLETED
    </label>
    <label>
      <input v-model="selectedFilter" value="pending" type="checkbox" />PENDING
    </label>
    <label><input v-model="selectedFilter" value="all" type="checkbox" />ALL </label>
  </div> -->
  <FilterTask />

  <div class="searchTodos">
  <input v-model="searchText" type="text" placeholder="SEARCH TODOS..." />
  </div>

  <div v-for="(task, index) in taskClone" :key="index">
    <div
      style="
        display: flex;
        flex-direction: row;
        align-items: center;
        margin-bottom: 10px;
      "
    >
      <div
        class="task-text"
        :style="{
          textDecoration: task.completed ? 'line-through' : 'none',
          textDecorationThickness: '3px',
        }"
      >
        {{ task.text }}
      </div>

      <button
        class="markcomplete-btn"
        v-if="!task.completed"
        @click="markComplete(task.id)"
      >
        MARK AS COMPLETE
      </button>
      <button class="completed-btn" v-else disabled>COMPLETED</button>
      <button class="delete-btn" @click="deleteTask(task.id)">DELETE</button>
    </div>
  </div>
</template>

<script>
import {useTodoStore} from '@/stores/todo.js'
import { mapState } from 'pinia';
import FilterTask from './FilterTask.vue';

export default {
  components: {
    FilterTask
  },
  data() {
    return {
      newTask: "",
      searchText : "",
      // selectedFilter : ["all"],
      tasks: [],
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push({
          text: this.newTask,
          completed: false,
          id: new Date().getTime()
        });
        this.newTask = "";
        
      } 
    },

    markComplete(id) {
      let index = this.tasks.findIndex(task => task.id === id);
      this.tasks[index].completed = true;
    },

    deleteTask(id) {
      let index = this.tasks.findIndex(task => task.id === id);
      this.tasks.splice(index, 1);
    },

    
  },

  computed: {
    ...mapState(useTodoStore, ["selectedFilter"]),
    taskClone() {
      let result = [];     
    
      for (let i = 0; i < this.tasks.length; i++) {
        
        let taskName = this.tasks[i].text;
        if(this.selectedFilter.includes("all") && taskName.includes(this.searchText)) {
          result.push(this.tasks[i]);
          continue;
         
        }
        if (this.tasks[i].completed === true && this.selectedFilter.includes("completed") && taskName.includes(this.searchText)) {
          result.push(this.tasks[i]);
        }
        if (
          this.tasks[i].completed !== true && this.selectedFilter.includes("pending") && taskName.includes(this.searchText)
        ) {
          result.push(this.tasks[i]);
        }
      }
      return result;
    },
  },
};
</script> 

<style>
body {
  margin-top: 80px;
  margin-left: 80px;
}
input[type="text"] {
  width: 280px;
  padding: 13px;
  margin-right: 12px;
  margin-bottom: 42px;
  border-radius: 10px;
  border: 2px solid black;
  text-transform: uppercase;
  font-family: Arial, Helvetica, sans-serif;
}

.task-text {
  display: inline-block;
  border-radius: 10px;
  width: 300px;
  border: 2px solid black;
  margin-right: 12px;
  padding: 6px 10px;
  text-align: center;
  text-transform: uppercase;
  font-family: Arial, Helvetica, sans-serif;
  font-size: small;
}

.add-btn {
  background-color: white;
  padding: 12px;   
  border-radius: 10px;
  width: 120px;
  border: 2px solid black;
}

.delete-btn {
  background-color: white;
  height: 35px;
  border-radius: 10px;
  width: 90px;
  border: 2px solid black;
}

.markcomplete-btn {
  background-color: white;
  width: 180px;
  border-radius: 10px;
  border: 2px solid black;
  padding: 10px;
  margin-right: 12px;
}

.completed-btn {
  background-color: rgb(166, 184, 205);
  color: black;
  width: 180px;
  border-radius: 10px;
  border: 2px solid black;
  padding: 12px;
  margin-right: 12px;
  text-decoration-thickness: bold;
}

.searchTodos{
  margin-top: 20px;
}

.filters {
  display: flex;
  gap: 30px;
}

label {
  display: inline-flex;
  align-items: center;
  gap: 5px;
}

</style>
