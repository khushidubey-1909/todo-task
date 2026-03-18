<template>
  <input v-model="newTask" type="text" placeholder="ENTER YOUR TASK HERE..." />
  <button class="add-btn" @click="addTask">ADD</button>
  <div v-for="(task, index) in tasks" :key="index">
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
          textDecoration: task.completed ? 'line-through' : 'none', textDecorationThickness: '3px',
        }"
      >
        {{ task.text }}
      </div>

      <button
        class="markcomplete-btn"
        v-if="!task.completed"
        @click="markComplete(index)"
      >
        MARK AS COMPLETE
      </button>
      <button class="completed-btn" v-else disabled>COMPLETED</button>
      <button class="delete-btn" @click="deleteTask(index)">DELETE</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [],
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push({
          text: this.newTask,
          completed: false,
        });
        this.newTask = "";
      }
    },

    markComplete(index) {
      this.tasks[index].completed = true;
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
  },
};
</script>

<style>
body {
  margin-top: 80px;
  margin-left: 80px;
}
input {
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


</style>
