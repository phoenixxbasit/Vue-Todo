<template>
  <div class="main">
    <form @submit.prevent="submit">
      <label for="name">Name</label>
      <input type="text" v-model.trim="name" id="name" name="name" />

      <label for="email">Email</label>
      <input type="text" v-model.trim="email" id="email" name="email" />

      <label for="password">Password</label>
      <input type="password" v-model.trim="password" id="password" name="password" />

      <label for="cpassword">Confirm Passowrd</label>
      <input type="password" v-model.trim="cpassword" id="cpassword" name="cpassword" />

      <button type="submit">Submit</button>
      <p v-if="showerror">{{ message }}</p>
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      name: "",
      email: "",
      password: "",
      cpassword: "",
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

    submit() {
      if (this.password !== this.cpassword) {
        this.showerror = true;
        this.message = "Email or Password is Incorrect";
        this.reset();
      } else {
        fetch("https://hvturufdwahrwbcywqbz.supabase.co/rest/v1/users", {
          method: "POST",
          headers: {
            apikey: import.meta.env.VITE_SUPABASE_API,
            Authorization: import.meta.env.VITE_SUPABASE_AUTHORIZATION,
            prefer: "return=representation",
            "Content-Type": "application/json"
          },
          body: JSON.stringify({
            name: this.name,
            email: this.email,
            password: this.password
          })
        })
          .then((res) => {
            if (!res.ok) {
              throw new Error("Network response was not ok");
            }
            return res.json();
          })
          .then((data) => {
            localStorage.setItem("Auth", JSON.stringify(data[0]));
            this.$router.push("/");
          })
          .catch((err) => {
            console.error("Error:", err);
          });
      }
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
</style>
