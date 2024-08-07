<!-- src/App.vue -->
<template>
  <div id="app">
    <Navbar />
    <router-view></router-view>
  </div>
</template>

<script setup>
// Import the Navbar component
import Navbar from './components/NavBar.vue'
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
