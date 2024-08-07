<template>
  <div class="cart">
    <h1>Shopping Cart</h1>
    <ul>
      <li v-for="item in cartItems" :key="item.id" class="cart-item">
        <span>{{ item.name }}</span>
        <span>${{ item.price.toFixed(2) }}</span>
        <button @click="removeFromCart(item)">Remove</button>
      </li>
    </ul>
    <p v-if="cartItems.length === 0">Your cart is empty.</p>
    <p v-else>Total: ${{ totalPrice.toFixed(2) }}</p>

    <button @click="clearCart" :disabled="cartItems.length === 0">
      Clear Cart
    </button>
  </div>
</template>

<script>
export default {
  name: "Cart",
  data() {
    return {
      cartItems: [
        // Example items
        { id: 1, name: "Item 1", price: 10.0 },
        { id: 2, name: "Item 2", price: 20.0 },
      ],
    };
  },
  computed: {
    totalPrice() {
      return this.cartItems.reduce((total, item) => total + item.price, 0);
    },
  },
  methods: {
    removeFromCart(item) {
      this.cartItems = this.cartItems.filter(
        (cartItem) => cartItem.id !== item.id
      );
    },
    clearCart() {
      this.cartItems = [];
    },
  },
};
</script>

<style scoped>
.cart {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
}

.cart h1 {
  font-size: 1.5rem;
  margin-bottom: 10px;
  text-align: center;
}

.cart ul {
  list-style-type: none;
  padding: 0;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.cart-item span {
  font-size: 1rem;
}

.cart-item button {
  padding: 5px 10px;
  font-size: 0.9rem;
  background-color: #e57373;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.cart-item button:hover {
  background-color: #d32f2f;
}

button:disabled {
  background-color: #bdbdbd;
  cursor: not-allowed;
}

button {
  margin-top: 10px;
  padding: 10px 15px;
  font-size: 1rem;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover:not(:disabled) {
  background-color: #36a273;
}
</style>
