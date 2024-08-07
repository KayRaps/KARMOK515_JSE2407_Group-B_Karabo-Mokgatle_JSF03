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

const props = defineProps({
  product: Object,
  isFavorite: Boolean,
  toggleFavorite: Function,
})

const toggleFavorite = () => {
  props.toggleFavorite(props.product.id)
}
</script>

<style scoped>
.border {
  border: 1px solid #e0e0e0;
}
</style>
