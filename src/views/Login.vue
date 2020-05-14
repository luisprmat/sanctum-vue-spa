<template>
  <div>
    <form @submit.prevent="login">
      <input v-model="form.email" type="email" placeholder="Email ..." /><br />
      <input v-model="form.password" type="password" placeholder="Contraseña ..."><br>
      <button>Login</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        email: "luisprmat@gmail.com",
        password: "password"
      }
    };
  },
  methods: {
    async login() {
			await this.$store.dispatch('login', this.form)
			return this.$router.replace('/');
    },
  }
};
</script>