<!-- Wishlist.vue -->
<template>
    <div>
      <h1>Your Wishlist</h1>
      <!-- Wishlist content goes here -->
    </div>
  </template>
  
  <script>
  export default {
    name: 'Wishlist',
    data() {
      return {
        items: []
      };
    }
  };
  </script>
  
  <style scoped>
  h1 {
    color: #42b983;
  }
  </style>
  