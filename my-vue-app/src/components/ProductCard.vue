<!-- src/components/ProductCard.vue -->
<template>
  <div class="border p-4 rounded-lg shadow-lg">
    <router-link :to="`/product/${product.id}`">
      <img :src="product.image" alt="Product image" class="w-full h-48 object-cover" />
      <h3 class="text-xl font-bold mt-4">{{ product.title }}</h3>
    </router-link>
    <p class="text-gray-700">{{ '$' + product.price }}</p>
    <p class="text-gray-500">{{ product.category }}</p>
    <p class="text-gray-600 mt-2">{{ product.description }}</p>
    <div class="flex justify-between items-center mt-4">
      <button @click="toggleFavorite" class="bg-blue-600 px-4 py-1 text-white rounded">
        {{ isFavorite ? 'Remove from Favorites' : 'Add To Favorites' }}
      </button>
      <p class="text-gray-700">Rating: {{ product.rating.rate }} ({{ product.rating.count }} reviews)</p>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

/**
 * Props for the ProductCard component
 * @typedef {Object} Props
 * @property {Object} product - The product object containing details to display
 * @property {string} product.id - The unique identifier for the product
 * @property {string} product.image - The URL of the product image
 * @property {string} product.title - The title of the product
 * @property {number} product.price - The price of the product
 * @property {string} product.category - The category of the product
 * @property {string} product.description - The description of the product
 * @property {Object} product.rating - The rating object for the product
 * @property {number} product.rating.rate - The average rating value
 * @property {number} product.rating.count - The number of reviews
 * @property {boolean} isFavorite - A flag indicating if the product is a favorite
 * @property {Function} toggleFavorite - Function to toggle the favorite status of the product
 */

const props = defineProps({
  product: Object,
  isFavorite: Boolean,
  toggleFavorite: Function,
})

/**
 * Toggles the favorite status of the product
 */
const toggleFavorite = () => {
  props.toggleFavorite(props.product.id)
}
</script>

<style scoped>
.border {
  border: 1px solid #e0e0e0;
}
</style>
