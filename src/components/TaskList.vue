<template>
  <div class="add-task">
    <form @submit.prevent="createTask">
      <input type="text" name="title" id="title" v-model="task.title" />
      <textarea
        name="description"
        id="description"
        cols="30"
        rows="10"
        v-model="task.description"
      ></textarea>
      <button type="submit">{{ mode == "C" ? "Submit" : "Update" }}</button>
    </form>
  </div>
  <div v-if="tasks.length" class="task-list">
    <div v-for="task in tasks" :key="task.id" class="task-item">
      <SingleTask :task="task" @delete="deleteTask" @update="updateTask" />
    </div>
  </div>
  <div v-else>
    <h2>You Are All Done.</h2>
  </div>
</template>

<script>
import SingleTask from "./SingleTask.vue";
export default {
  components: {
    SingleTask
  },
  data() {
    return {
      task: {
        title: "",
        description: "",
        completed: false,
        user: 0
      },
      tasks: [],
      mode: "C"
    };
  },

  mounted() {
    this.task.user = JSON.parse(localStorage.getItem("Auth")).id;
    fetch(
      `https://hvturufdwahrwbcywqbz.supabase.co/rest/v1/tasks?user=eq.${this.task.user}&select=*`,
      {
        headers: {
          apikey: import.meta.env.VITE_SUPABASE_API,
          Authorization: import.meta.env.VITE_SUPABASE_AUTHORIZATION
        }
      }
    )
      .then((res) => res.json())
      .then((data) => (this.tasks = data))
      .catch((err) => console.log(err.message));
  },
  methods: {
    createTask() {
      if (this.mode == "C") {
        fetch("https://hvturufdwahrwbcywqbz.supabase.co/rest/v1/tasks", {
          method: "POST",
          headers: {
            apikey: import.meta.env.VITE_SUPABASE_API,
            Authorization: import.meta.env.VITE_SUPABASE_AUTHORIZATION,
            "Content-Type": "application/json"
          },
          body: JSON.stringify(this.task)
        })
          .then((res) => {
            console.log(res);
            this.tasks.push(this.task);
            this.task = {
              title: "",
              description: "",
              completed: false
            };
          })
          .catch((err) => console.log(err));
      } else {
        fetch(`https://hvturufdwahrwbcywqbz.supabase.co/rest/v1/tasks?id=eq.${this.task.id}`, {
          method: "PATCH",
          headers: {
            apikey: import.meta.env.VITE_SUPABASE_API,
            Authorization: import.meta.env.VITE_SUPABASE_AUTHORIZATION,
            "Content-Type": "application/json"
          },
          body: JSON.stringify(this.task)
        })
          .then((res) => {
            console.log(res);

            this.task = {
              title: "",
              description: "",
              completed: false
            };
          })
          .catch((err) => console.log(err));
        this.mode = "C";
      }
    },
    updateTask(id) {
      this.mode = "U";
      this.task = this.tasks.find((task) => task.id === id);
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((project) => project.id !== id);
    }
  }
};
</script>

<style scoped>
.add-task {
  display: flex;
  flex-direction: column;
  background: var(--color-background);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 20px;
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

input,
textarea {
  color: white;
  border: 1px solid var(--color-border);
  border-radius: 4px;
  padding: 10px;
  background: linear-gradient(135deg, var(--color-background-soft), var(--color-background));
}

button[type="submit"] {
  background: hsla(160, 100%, 37%, 1);
  border: 1px solid var(--color-border);
  border-radius: 4px;
  padding: 10px;
  cursor: pointer;
  text-align: center;
}

.task-list {
  display: flex;
  flex-direction: column;
}

.task-item {
  border: 1px solid var(--color-border);
  border-radius: 8px;
  padding: 10px;
  margin-bottom: 10px;
}
</style>
