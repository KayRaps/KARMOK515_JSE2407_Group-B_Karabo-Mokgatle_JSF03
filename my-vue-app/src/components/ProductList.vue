<template>
  <div class="container mx-auto p-6">
    <div class="flex justify-between mb-4">
      <select
        v-model="selectedCategory"
        @change="filterProducts"
        class="border p-2 rounded"
      >
        <option value="">All Categories</option>
        <option v-for="category in categories" :key="category" :value="category">
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
        <button @click="filterProducts" class="bg-blue-500 text-white p-2 rounded-r">
          Search
        </button>
      </div>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <div
        v-for="product in filteredProducts"
        :key="product.id"
        class="bg-white rounded-lg shadow-md overflow-hidden"
      >
        <img :src="product.image" alt="product" class="w-full h-48 object-cover" />
        <div class="p-4">
          <h3 class="text-lg font-semibold">{{ product.name }}</h3>
          <p class="text-gray-500">{{ product.description }}</p>
          <p class="text-blue-500 font-bold">R{{ product.price }}</p>
          <router-link
            :to="{ name: 'ProductDetails', params: { id: product.id }}"
            class="mt-2 bg-blue-500 text-white p-2 rounded inline-block"
          >
            View Details
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  /**
   * The component's data function.
   * @returns {Object} The data properties for the component.
   */
  data() {
    return {
      /** @type {string} The search query entered by the user. */
      searchQuery: "",

      /** @type {string} The currently selected category. */
      selectedCategory: "",

      /** @type {Array<string>} List of available categories. */
      categories: ["Category 1", "Category 2", "Category 3"],

      /**
       * @type {Array<Object>} List of products available for display.
       * @property {number} id - The unique identifier for the product.
       * @property {string} name - The name of the product.
       * @property {string} description - A brief description of the product.
       * @property {number} price - The price of the product.
       * @property {string} image - The URL of the product image.
       * @property {string} category - The category of the product.
       */
      products: [
        {
          id: 1,
          name: "Product 1",
          description: "Description for Product 1",
          price: 100,
          image: "src/assets/image1.jpg",
          category: "Category 1",
        },
        {
          id: 2,
          name: "Product 2",
          description: "Description for Product 2",
          price: 200,
          image: "src/assets/image2.jpg",
          category: "Category 2",
        },
        {
          id: 3,
          name: "Product 3",
          description: "Description for Product 3",
          price: 300,
          image: "src/assets/image3.jpg",
          category: "Category 1",
        },
        {
          id: 4,
          name: "Product 4",
          description: "Description for Product 4",
          price: 400,
          image: "src/assets/image4.jpg",
          category: "Category 3",
        },
      ],
    };
  },

  computed: {
    /**
     * Filters the products based on the selected category and search query.
     * @returns {Array<Object>} The list of products that match the filter criteria.
     */
    filteredProducts() {
      return this.products.filter((product) => {
        const matchesCategory =
          this.selectedCategory === "" ||
          product.category === this.selectedCategory;
        const matchesSearch =
          this.searchQuery === "" ||
          product.name.toLowerCase().includes(this.searchQuery.toLowerCase());
        return matchesCategory && matchesSearch;
      });
    },
  },

  methods: {
    /**
     * Triggers the filtering of products based on the current search query and selected category.
     */
    filterProducts() {
      // Logic for filtering products
    },
  },
};
</script>
