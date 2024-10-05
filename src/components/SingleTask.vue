<template>
  <div class="main-task" @click="revealtoggle">
    <div class="task-content">
      {{ task.title }}
      <span @click="taskDelete" class="material-icons">delete</span>
    </div>
    <div class="description" v-if="show">
      {{ task.description }}
      <span>
        {{ task.completed }}
      </span>

      <button @click="taskupdate">Update</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["task"],
  data() {
    return {
      show: false
    };
  },
  methods: {
    revealtoggle() {
      this.show = !this.show;
    },
    taskDelete() {
      console.log(this.task.user);
      fetch(`https://hvturufdwahrwbcywqbz.supabase.co/rest/v1/tasks?id=eq.${this.task.user}`, {
        method: "DELETE",
        headers: {
          apikey: import.meta.env.VITE_SUPABASE_API,
          Authorization: import.meta.env.VITE_SUPABASE_AUTHORIZATION
        }
      })
        .then((res) => {
          if (res.ok) {
            this.$emit("delete", this.task.id);
          } else {
            console.error(res);
          }
        })
        .catch((err) => console.log(err.message));
    },
    taskupdate() {
      this.$emit("update", this.task.id);
    }
  }
};
</script>

<style scoped>
.main-task {
  background: var(--color-background);
  border-radius: 8px;
  padding: 10px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: background 0.3s;
}

.main-task:hover {
  background: var(--color-background-soft);
}

.task-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: var(--color-text);
}

.task-content .material-icons {
  cursor: pointer;
  color: hsla(160, 100%, 37%, 1);
}

.description {
  padding: 10px;
  background: linear-gradient(135deg, var(--color-background-soft), var(--color-background));
  border: 1px solid var(--color-border);
  border-radius: 4px;
  color: var(--color-text);
  margin-top: 10px;
  display: flex;
  justify-content: space-between;
  cursor: initial;
}

.description button {
  background: var(--color-background-soft);
  border: 1px solid var(--color-border);
  border-radius: 4px;
  padding: 10px;
  cursor: pointer;
  text-align: center;
  color: var(--color-text);
}

.description span button {
  background: var(--color-background-soft);
  border: 1px solid var(--color-border);
  border-radius: 4px;
  padding: 10px;
  cursor: pointer;
  text-align: center;
  color: var(--color-text);
}
</style>
