<template>
  <div>
    <h1>{{ product.title }}</h1>
    <img :src="product.image" alt="Product Image" />
    <p>Price: {{ product.price }}</p>
    <p>{{ product.description }}</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const product = ref({});

onMounted(async () => {
  const response = await fetch(`https://fakestoreapi.com/products/${route.params.id}`);
  product.value = await response.json();
});
</script>

<style scoped>
/* Add any styles here */
</style>
