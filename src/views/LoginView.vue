<template>
  <div class="main">
    <form @submit.prevent="submit">
      <label for="email">Email</label>
      <input type="text" v-model="email" id="email" name="email" />

      <label for="password">Password</label>
      <input type="password" v-model="password" id="password" name="password" />

      <button type="submit">Submit</button>
      <button @click.left="navigateToRegister" class="register">Register</button>

      <p v-if="showerror">{{ message }}</p>
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      showerror: false,
      message: ""
    };
  },

  methods: {
    reset() {
      setTimeout(() => {
        this.showerror = false;
      }, 2000);
    },

    navigateToRegister() {
      this.$router.push({ name: "register" });
    },

    submit() {
      fetch(
        `https://hvturufdwahrwbcywqbz.supabase.co/rest/v1/users?email=eq.${this.email}&select=*`,
        {
          headers: {
            apikey: import.meta.env.VITE_SUPABASE_API,
            Authorization: import.meta.env.VITE_SUPABASE_AUTHORIZATION
          }
        }
      )
        .then((res) => res.json())
        .then((data) => {
          if (data.lenth == 0 || this.password != data[0]?.password) {
            this.showerror = true;
            this.message = "Email or Password is Incorrect";
            this.reset();
          } else {
            localStorage.setItem("Auth", JSON.stringify(data[0]));
            this.$router.push({ name: "home" });
          }
        })
        .catch((err) => console.error(err));
    }
  }
};
</script>

<style scoped>
.main {
  display: flex;
  flex-direction: column;
  background: var(--color-background);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  padding: 20px;
  max-width: 300px;
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

input {
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
p {
  color: red;
}

.register {
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
