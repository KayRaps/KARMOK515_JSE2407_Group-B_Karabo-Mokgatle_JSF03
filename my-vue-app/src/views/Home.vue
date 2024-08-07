<!-- src/views/Home.vue -->
<template>
  <div class="container mx-auto p-6">
    <div class="flex justify-between mb-4">
      <select
        v-model="selectedCategory"
        @change="filterProducts"
        class="border p-2 rounded"
      >
        <option value="">All Categories</option>
        <option
          v-for="category in categories"
          :key="category"
          :value="category"
        >
          {{ category }}
        </option>
      </select>

      <div class="flex items-center">
        <input
          type="text"
          v-model="searchQuery"
          placeholder="Search products..."
          class="border p-2 rounded-l"
        />
        <button
          @click="filterProducts"
          class="bg-blue-500 text-black hover:bg-white border border-l-0 p-2 rounded-r"
        >
          Search
        </button>
      </div>

      <select
        v-model="sortOrder"
        @change="sortProducts"
        class="border p-2 rounded"
      >
        <option value="">Sort by Price</option>
        <option value="asc">Lowest to Highest</option>
        <option value="desc">Highest to Lowest</option>
      </select>
    </div>

    <div class="loading text-center text-2xl" v-if="loading">Loading...</div>

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
import { ref, onMounted } from "vue";
import ProductCard from "../components/ProductCard.vue";

/**
 * List of all products fetched from the API.
 * @type {Ref<Array>}
 */
const products = ref([]);

/**
 * List of products filtered by search and category.
 * @type {Ref<Array>}
 */
const filteredProducts = ref([]);

/**
 * List of product categories fetched from the API.
 * @type {Ref<Array>}
 */
const categories = ref([]);

/**
 * Search query entered by the user.
 * @type {Ref<string>}
 */
const searchQuery = ref("");

/**
 * Currently selected category for filtering products.
 * @type {Ref<string>}
 */
const selectedCategory = ref("");

/**
 * Current sort order for the product list.
 * @type {Ref<string>}
 */
const sortOrder = ref("");

/**
 * List of favorite product IDs stored in local storage.
 * @type {Ref<Array>}
 */
const favorites = ref(JSON.parse(localStorage.getItem("favorites")) || []);

/**
 * Indicates whether the data is currently being loaded.
 * @type {Ref<boolean>}
 */
const loading = ref(true);

/**
 * Fetches the list of products from the API and sets the `products` and `filteredProducts` arrays.
 * @returns {Promise<void>}
 */
const fetchProducts = async () => {
  loading.value = true;
  const response = await fetch("https://fakestoreapi.com/products");
  const data = await response.json();
  products.value = data;
  filteredProducts.value = data;
  loading.value = false;
};

/**
 * Fetches the list of product categories from the API and sets the `categories` array.
 * @returns {Promise<void>}
 */
const fetchCategories = async () => {
  const response = await fetch("https://fakestoreapi.com/products/categories");
  const data = await response.json();
  categories.value = data;
};

/**
 * Filters the list of products based on the search query and selected category.
 */
const filterProducts = () => {
  loading.value = true;
  filteredProducts.value = products.value.filter((product) => {
    const matchesSearch = product.title
      .toLowerCase()
      .includes(searchQuery.value.toLowerCase());
    const matchesCategory =
      !selectedCategory.value || product.category === selectedCategory.value;
    return matchesSearch && matchesCategory;
  });
  sortProducts();
  loading.value = false;
};

/**
 * Sorts the list of filtered products based on the current sort order.
 */
const sortProducts = () => {
  if (sortOrder.value) {
    filteredProducts.value.sort((a, b) => {
      if (sortOrder.value === "asc") {
        return a.price - b.price;
      } else if (sortOrder.value === "desc") {
        return b.price - a.price;
      }
      return 0;
    });
  }
};

/**
 * Toggles the favorite status of a product.
 * If the product is already a favorite, it is removed; otherwise, it is added.
 * @param {number} productId - The ID of the product to toggle.
 */
const toggleFavorite = (productId) => {
  if (favorites.value.includes(productId)) {
    favorites.value = favorites.value.filter((id) => id !== productId);
  } else {
    favorites.value.push(productId);
  }
  localStorage.setItem("favorites", JSON.stringify(favorites.value));
};

/**
 * Checks if a product is marked as a favorite.
 * @param {number} productId - The ID of the product to check.
 * @returns {boolean} True if the product is a favorite, false otherwise.
 */
const isFavorite = (productId) => {
  return favorites.value.includes(productId);
};

/**
 * Lifecycle hook that fetches categories and products when the component is mounted.
 */
onMounted(async () => {
  await fetchCategories();
  await fetchProducts();
});
</script>

<style scoped>
.grid {
  display: grid;
  gap: 1rem;
}
</style>
