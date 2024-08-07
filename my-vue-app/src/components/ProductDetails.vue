<template>
  <div class="container mx-auto p-6">
    <div v-if="product" class="max-w-2xl mx-auto bg-white rounded-lg shadow-md overflow-hidden">
      <img :src="product.image" alt="product" class="w-full h-64 object-cover" />
      <div class="p-4">
        <h3 class="text-2xl font-semibold">{{ product.name }}</h3>
        <p class="text-gray-500">{{ product.description }}</p>
        <p class="text-blue-500 font-bold text-lg">R{{ product.price }}</p>
        <button @click="addToCart(product)" class="mt-2 bg-blue-500 text-white p-2 rounded">
          Add to Cart
        </button>
      </div>
    </div>
    <div v-else class="text-center">
      <p>Product not found.</p>
      <router-link to="/" class="text-blue-500">Go back to products</router-link>
    </div>
  </div>
</template>

<script>
export default {
  /**
   * @module ProductDetails
   * @description A Vue component for displaying product details and allowing users to add products to their cart.
   */
  data() {
    return {
      /**
       * @typedef {Object} Product
       * @property {number} id - The unique identifier for the product.
       * @property {string} name - The name of the product.
       * @property {string} description - A brief description of the product.
       * @property {number} price - The price of the product in Rands.
       * @property {string} image - The URL to the product image.
       * @property {string} category - The category the product belongs to.
       */

      /**
       * @type {Product[]}
       * @description An array of available products.
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
     * @description Computes the current product based on the route parameter.
     * @returns {Product | undefined} The current product object or undefined if not found.
     */
    product() {
      const productId = parseInt(this.$route.params.id);
      return this.products.find((product) => product.id === productId);
    },
  },
  methods: {
    /**
     * @description Adds the specified product to the shopping cart.
     * @param {Product} product - The product to add to the cart.
     */
    addToCart(product) {
      console.log(`Added ${product.name} to cart`);
    },
  },
};
</script>
