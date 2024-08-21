<template>
  <div class="container">
    <h1 class="title">Todo List Geor</h1>
    <form @submit.prevent="addTask" class="form">
      <input v-model="newTask" placeholder="Nueva tarea..." class="input" />
      <button type="submit" class="submit">
        <svg
          class="svg"
          aria-hidden="true"
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          fill="none"
          viewBox="0 0 24 24"
        >
          <path
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M12 6v13m0-13 4 4m-4-4-4 4"
          />
        </svg>
      </button>
    </form>

    <div class="container-tasks">
      <ul>
        <li v-for="task in tasks" :key="task.id" class="item">
          <div class="b-done-task">
            <button @click="toggleTask(task.id)" class="button-icon">
              <svg
                class="w-6 h-6 text-gray-800 dark:text-white"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                fill="none"
                viewBox="0 0 24 24"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M8.5 11.5 11 14l4-4m6 2a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
                />
              </svg>
            </button>
            <span
              :style="{
                textDecoration: task.completed ? 'line-through' : 'none',
              }"
            >
              {{ task.title }}
            </span>
          </div>

          <div class="b-edit-delete">
            <button @click="editTask(task.id)" class="button-icon">
              <svg
                class="w-6 h-6 text-gray-800 dark:text-white"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                fill="none"
                viewBox="0 0 24 24"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="m16 10 3-3m0 0-3-3m3 3H5v3m3 4-3 3m0 0 3 3m-3-3h14v-3"
                />
              </svg>
            </button>
            <button @click="deleteTask(task.id)" class="button-icon">
              <svg
                class="w-6 h-6 text-gray-800 dark:text-white"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                fill="currentColor"
                viewBox="0 0 24 24"
              >
                <path
                  fill-rule="evenodd"
                  d="M8.586 2.586A2 2 0 0 1 10 2h4a2 2 0 0 1 2 2v2h3a1 1 0 1 1 0 2v12a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V8a1 1 0 0 1 0-2h3V4a2 2 0 0 1 .586-1.414ZM10 6h4V4h-4v2Zm1 4a1 1 0 1 0-2 0v8a1 1 0 1 0 2 0v-8Zm4 0a1 1 0 1 0-2 0v8a1 1 0 1 0 2 0v-8Z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
          </div>
        </li>
      </ul>
    </div>

    <div v-if="editingTask" class="update">
      <input v-model="editingTask.title" class="input" />
      <button @click="updateTask" class="button-icon">
        <svg
          class="w-6 h-6 text-gray-800 dark:text-white"
          aria-hidden="true"
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          fill="none"
          viewBox="0 0 24 24"
        >
          <path
            stroke="currentColor"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="m16 10 3-3m0 0-3-3m3 3H5v3m3 4-3 3m0 0 3 3m-3-3h14v-3"
          />
        </svg>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      newTask: "",
      editingTask: null,
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() === "") return;
      this.tasks.push({
        id: Date.now(),
        title: this.newTask,
        completed: false,
      });
      this.newTask = "";
    },
    toggleTask(id) {
      const task = this.tasks.find((task) => task.id === id);
      if (task) task.completed = !task.completed;
    },
    editTask(id) {
      this.editingTask = { ...this.tasks.find((task) => task.id === id) };
    },
    updateTask() {
      const index = this.tasks.findIndex(
        (task) => task.id === this.editingTask.id
      );
      if (index !== -1) {
        this.tasks.splice(index, 1, this.editingTask);
        this.editingTask = null;
      }
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1em 0;
}

.title {
  color: var(--color-gray);
  margin-bottom: 0.5em;
}

.form {
  display: flex;
  gap: 7px;
  background-color: var(--color-dark-dif);
  padding: 1em;
  border-radius: 0.5em;
  margin-bottom: 2rem;
}

.update {
  display: flex;
  gap: 7px;
  margin-top: .8em;
  background-color: rgb(72, 100, 201);
  padding: .5em;
  border-radius: 0.5em;
  margin-bottom: 2rem;

}

.input {
  border-radius: inherit;
  padding: 0.5em;
  width: 320px;
  background-color: inherit;
  border: 1px solid var(--color-gray-hard);
  color: var(--color-gray);
  font-size: 1rem;
}

.input:focus {
  outline: none;
}

.submit {
  background-color: var(--color-button);
  border: none;
  border-radius: inherit;
  padding: 0 0.4em;
  cursor: pointer;
}

.svg {
  color: white;
}

ul {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 385px;
  border: 1px solid var(--color-gray-hard);
  border-radius: 0.3em;
  padding: 0.5em;
}
.container-tasks {
  background-color: inherit;
  color: var(--color-gray-hard);
  padding: 0.3em;
}

.b-done-task {
  display: flex;
  align-items: center;
  gap: 10px;
}

.b-edit-delete {
  display: flex;
  gap: 10px;
}

svg {
  color: var(--color-gray-hard);
  width: 20px;
}

.button-icon {
  background-color: inherit;
  border: none;
  display: flex;
  align-items: center;
  cursor: pointer;
}
</style>