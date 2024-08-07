<!-- src/views/Home.vue -->
<template>
  <div class="container mx-auto p-6">
    <div class="flex justify-between mb-4">
      <select v-model="selectedCategory" @change="filterProducts" class="border p-2 rounded">
        <option value="">All Categories</option>
        <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
      </select>

      <div class="flex items-center">
        <input
          type="text"
          v-model="searchQuery"
          placeholder="Search products..."
          class="border p-2 rounded-l"
        />
        <button @click="filterProducts" class="bg-blue-500 text-black hover:bg-white border border-l-0 p-2 rounded-r">
          Search
        </button>
      </div>

      <select v-model="sortOrder" @change="sortProducts" class="border p-2 rounded">
        <option value="">Sort by Price</option>
        <option value="asc">Lowest to Highest</option>
        <option value="desc">Highest to Lowest</option>
      </select>
    </div>

    <div class="loading text-center text-2xl" v-if="loading">
      Loading...
    </div>

    <div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-4" v-else>
      <ProductCard
        v-for="product in filteredProducts"
        :key="product.id"
        :product="product"
        :isFavorite="isFavorite(product.id)"
        :toggleFavorite="toggleFavorite"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import ProductCard from '../components/ProductCard.vue'

const products = ref([])
const filteredProducts = ref([])
const categories = ref([])
const searchQuery = ref('')
const selectedCategory = ref('')
const sortOrder = ref('')
const favorites = ref(JSON.parse(localStorage.getItem('favorites')) || [])
const loading = ref(true)

const fetchProducts = async () => {
  loading.value = true
  const response = await fetch('https://fakestoreapi.com/products')
  const data = await response.json()
  products.value = data
  filteredProducts.value = data
  loading.value = false
}

const fetchCategories = async () => {
  const response = await fetch('https://fakestoreapi.com/products/categories')
  const data = await response.json()
  categories.value = data
}

const filterProducts = () => {
  loading.value = true
  filteredProducts.value = products.value.filter((product) => {
    const matchesSearch = product.title.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchesCategory = !selectedCategory.value || product.category === selectedCategory.value
    return matchesSearch && matchesCategory
  })
  sortProducts()
  loading.value = false
}

const sortProducts = () => {
  if (sortOrder.value) {
    filteredProducts.value.sort((a, b) => {
      if (sortOrder.value === 'asc') {
        return a.price - b.price
      } else if (sortOrder.value === 'desc') {
        return b.price - a.price
      }
      return 0
    })
  }
}

const toggleFavorite = (productId) => {
  if (favorites.value.includes(productId)) {
    favorites.value = favorites.value.filter((id) => id !== productId)
  } else {
    favorites.value.push(productId)
  }
  localStorage.setItem('favorites', JSON.stringify(favorites.value))
}

const isFavorite = (productId) => {
  return favorites.value.includes(productId)
}

onMounted(async () => {
  await fetchCategories()
  await fetchProducts()
})
</script>

<style scoped>
.grid {
  display: grid;
  gap: 1rem;
}
</style>
