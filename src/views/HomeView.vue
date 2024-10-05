<template>
  <button @click.left="reset">Logout</button>
  <main>
    <Tasks />
  </main>
</template>

<script>
import Tasks from "../components/TaskList.vue";
export default {
  components: {
    Tasks
  },
  methods: {
    reset() {
      localStorage.clear();
      this.$router.go("/");
    }
  }
};
</script>

<style scoped>
button {
  background: hsla(160, 100%, 37%, 1);
  border: 1px solid var(--color-border);
  border-radius: 4px;
  padding: 10px;
  cursor: pointer;
  text-align: center;
  position: fixed;
  width: 10%;
  top: 10px;
  right: 10px;
}
</style>
