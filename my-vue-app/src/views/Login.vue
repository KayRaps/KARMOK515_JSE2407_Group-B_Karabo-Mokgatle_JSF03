<!-- Login.vue -->
<template>
    <div>
      <h1>Login</h1>
      <form @submit.prevent="handleLogin">
        <input v-model="username" placeholder="Username" />
        <input type="password" v-model="password" placeholder="Password" />
        <button type="submit">Login</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Login',
    data() {
      return {
        username: '',
        password: ''
      };
    },
    methods: {
      async handleLogin() {
        try {
          // Assume some async operation here, e.g., an API call
          const response = await someApiCall(this.username, this.password);
          console.log(response);
        } catch (error) {
          console.error('Login error:', error);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  h1 {
    color: #333;
  }
  form {
    display: flex;
    flex-direction: column;
  }
  input {
    margin: 5px 0;
  }
  button {
    margin-top: 10px;
  }
  </style>
  