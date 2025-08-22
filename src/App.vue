<template>
  <h1>Shopping Cart</h1>
  <div class="container">
    <ProductList @add-to-cart="addToCart" />
    <div class="cart-wrapper">
      <ProductCart
        :cart="cart"
        @increase-qty="increaseQty"
        @decrease-quantity="decreaseQuantity"
        @remove-from-cart="removeFromCart"
        @clear-cart="clearCart"
      />
      <div v-if="showModal" class="modal-window">
        You added item to your shopping cart.
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted } from "vue";
import ProductList from "./components/ProductList.vue";
import ProductCart from "./components/ProductCart.vue";

// Cart
const cart = ref([]);
const showModal = ref(false);

// Function to save the cart to localStorage
const saveCartToLocalStorage = () => {
  localStorage.setItem("cart", JSON.stringify(cart.value));
};

// Loading the cart from localStorage on startup
onMounted(() => {
  try {
    const savedCart = localStorage.getItem("cart");
    if (savedCart) {
      const parsed = JSON.parse(savedCart);
      cart.value = Array.isArray(parsed) ? parsed : [];
    }
  } catch (e) {
    cart.value = [];
  }
});

// Adding a product to the cart based on quantity
const addToCart = (productWithQty) => {
  const existingItem = Array.isArray(cart.value)
    ? cart.value.find((item) => item.id === productWithQty.id)
    : null;

  if (existingItem) {
    existingItem.quantity += productWithQty.quantity;
  } else {
    cart.value.push({ ...productWithQty });
  }

  saveCartToLocalStorage();
  showModal.value = true;
  setTimeout(() => {
    showModal.value = false;
  }, 2000);
};

// Change product quantity
const increaseQty = (item) => {
  item.quantity += 1;
  saveCartToLocalStorage();
};

const decreaseQuantity = (item) => {
  if (item.quantity > 1) {
    item.quantity -= 1;
  } else {
    removeFromCart(item);
  }
  saveCartToLocalStorage();
};

// Remove an item from the cart
const removeFromCart = (item) => {
  cart.value = cart.value.filter((cartItem) => cartItem.id !== item.id);
  saveCartToLocalStorage();
};

const clearCart = () => {
  cart.value = [];
  saveCartToLocalStorage();
};
</script>

<style lang="scss">
.container {
  margin-bottom: 50px;
}

.container,
h1 {
  display: flex;
  padding-left: 15px;
  padding-right: 15px;
}

h1 {
  margin-top: 20px;
  margin-bottom: 20px;
}

.modal-window {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  background-color: #28a745;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  z-index: 1000;
  animation: fadeInOut 2s ease-in-out;
}

//Mobile styles
@media screen and (max-width: 1024px) {
  .container {
    flex-direction: column-reverse;
  }

  .products {
    margin-top: 20px;
  }
}

//Desktop styles
@media screen and (min-width: 1025px) {
  .container,
  h1 {
    max-width: 1280px;
    margin-left: auto;
    margin-right: auto;
  }

  .products {
    padding-right: 4%;
    width: 100%;
  }

  .cart-wrapper {
    position: relative;
    min-width: 330px;
  }
}
</style>
