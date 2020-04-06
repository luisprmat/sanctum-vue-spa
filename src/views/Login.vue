<template>
  <div>
    <h1>Login</h1>
    <div v-if="logged">
      Usuario autenticado: {{ user.name }}<br />
      <button @click="logout">Logout</button>
    </div>
    <form v-else @submit.prevent="login">
      <input v-model="form.email" type="email" placeholder="Email ..." /><br />
      <input v-model="form.password" type="password" placeholder="Contraseña ..."><br>
      <button>Login</button>
    </form>
    <div v-if="logged">
      <button @click="getUsers">Ver todos los usuarios</button>
      <table>
        <thead>
          <th>ID</th>
          <th>Nombre</th>
          <th>Email</th>
        </thead>
        <tbody>
          <tr :key="user.id" v-for="user in users">
            <td v-text="user.id"></td>
            <td v-text="user.name"></td>
            <td v-text="user.email"></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
axios.defaults.withCredentials = true;
axios.defaults.baseURL = "http://localhost:8000";
export default {
  data() {
    return {
      logged: false,
      users: [],
      form: {
        email: "luisprmat@gmail.com",
        password: "password"
      }
    };
  },
  created() {
    if (this.logged) {
      this.getUsers();
    }
  },
  methods: {
    login() {
      axios.get("/sanctum/csrf-cookie").then(() => {
        axios.post('/login', this.form)
          .then(res => {
          this.user = res.data;
          this.logged = true;
        });
      });
    },
    logout() {
      axios.post("/logout").then(() => {
        this.user = {};
        this.logged = false;
      });
    },
    getUsers() {
      axios.get("/api/user").then(res => {
          this.users = res.data;
      }).catch(() => {});
    }
  }
};
</script>