<template>
  <div class="filters">
    <label>
      <input
        v-model="selectedFilter"
        value="completed"
        type="checkbox"
      />COMPLETED
    </label>
    <label>
      <input v-model="selectedFilter" value="pending" type="checkbox" />PENDING
    </label>
    <label
      ><input v-model="selectedFilter" value="all" type="checkbox" />ALL
    </label>
  </div>
</template>

<script>
import { useTodoStore } from "@/stores/todo.js";
import { mapWritableState } from "pinia";

export default {
  computed: {
    ...mapWritableState(useTodoStore, ["selectedFilter"]),
  },
};
</script>

<style>
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
