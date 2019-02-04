<template>
  <div id="app">
    <div id="login" v-if="showLogin">
      <h1>Login</h1>
      <form>
        <label for="login.username">Username</label><br>
        <input type="text" id="login.username" v-model="username"/>

        <br><br>

        <label for="login.password">Password</label><br>
        <input type="password" id="login.password" v-model="password"/>

        <br><br>

        <button type="button" @click="login">Login</button><br><br>
        <button type="button" @click="openRegister">Register</button>
      </form>
    </div>

    <div id="home" v-if="showHome">
      <h1>Home</h1>

      <button type="button" @click="logout">Logout</button>
    </div>

    <div id="register" v-if="showRegister">
        <h1>Register</h1>

        <form>
            <label for="register.username">Username</label><br>
            <input type="text" id="register.username" v-model="username"/>

            <br><br>

            <label for="register.email">E-mail</label><br>
            <input type="text" id="register.email" v-model="email"/>

            <br><br>

            <label for="register.firstname">First name</label><br>
            <input type="text" id="register.firstname" v-model="firstname"/>

            <br><br>

            <label for="register.lastname">Last name</label><br>
            <input type="text" id="register.lastname" v-model="lastname"/>

            <br><br>

            <button type="button" @click="register">Register</button>
        </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
const apiUri = 'https://ldap-node-jwt.herokuapp.com/api';

export default {
  name: 'app',
    data() {
        return {
            username: null,
            password: null,
            email: null,
            firstname: null,
            lastname: null,
            showLogin: true,
            showRegister: false,
            showHome: false,
            token: null,
        };
    },
    methods: {
        async login() {
            const res = await axios.post(`${apiUri}/users/login`, {
                username: this.username,
                password: this.password,
            });

            this.token = res.data.jwt;
            this.openHome();
        },
        async register() {
              await axios.post(`${apiUri}/users`, {
            username: this.username,
            email: this.email,
            firstname: this.firstname,
            lastname: this.lastname,
          });

          alert('Please check your emails to validate your account !');

          this.openLogin();
        },
        openRegister() {
          this.showRegister = true;
          this.showHome = false;
          this.showLogin = false;
        },
        openLogin() {
            this.showRegister = false;
            this.showHome = false;
            this.showLogin = true;
        },
        openHome() {
          this.showRegister = false;
          this.showHome = true;
          this.showLogin = false;
        },
        logout() {
            this.username = null;
            this.password = null;
            this.email = null;
            this.firstname = null;
            this.lastname = null;
            this.token  = null;

            this.openLogin();
        }
    },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
