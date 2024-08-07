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

/**
 * Reactive reference to store the product details.
 * @type {Object}
 */
const product = ref({});

/**
 * Provides access to the current route and its parameters.
 * @returns {Object} The current route.
 */
const route = useRoute();

/**
 * Fetches the product data based on the route parameter `id`
 * and assigns it to the `product` reference.
 * 
 * @async
 * @function fetchProduct
 * @returns {Promise<void>} A promise that resolves when the product data has been fetched.
 */
const fetchProduct = async () => {
  try {
    const response = await fetch(`https://fakestoreapi.com/products/${route.params.id}`);
    product.value = await response.json();
  } catch (error) {
    console.error('Error fetching product data:', error);
  }
};

onMounted(fetchProduct);
</script>

<style scoped>
/* Add any styles here */
</style>
