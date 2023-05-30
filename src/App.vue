<template>
  <div>
    <!-- heading -->
    <header>
      <img src="./assets/pinia-logo.svg" alt="pinia-logo" />
      <h1>Pinia Tasks</h1>
    </header>

    <!-- new task form -->

    <div class="new-task-form">
      <TaskForm />
    </div>
    <!-- filter -->
    <nav class="filter">
      <button @click="filter = 'all'">all tasks</button>
      <button @click="filter = 'favs'">favs tasks</button>
    </nav>

    <div class="loading" v-if="loading == true">Loading.....</div>

    <!-- task list -->
    <div class="task-list" v-if="filter == 'all'">
      <p>You have {{ totalCount }} tasks left to do</p>
      <div v-for="task in tasks">
        <TaskDetails :task="task" />
      </div>
    </div>

    <div class="task-list" v-if="filter == 'favs'">
      <p>You have {{ favCount }} favs left to do</p>
      <div v-for="task in favs">
        <TaskDetails :task="task" />
      </div>
    </div>

    <!-- reset -->
    <button @click="taskStore.$reset">reset state</button>
  </div>
</template>

<script>
import { storeToRefs } from "pinia";
import { useTaskStore } from "./stores/TaskStore";
import TaskDetails from "./components/TaskDetails.vue";
import { ref } from "vue";
import TaskForm from "./components/TaskForm.vue";

export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const taskStore = useTaskStore();

    const { tasks, loading, favs, totalCount, favCount } =
      storeToRefs(taskStore);
    // fetch data
    taskStore.getTasks();

    const filter = ref("all");
    console.log("this is app");
    return { taskStore, filter, tasks, loading, favs, totalCount, favCount };
  },
};
</script>
