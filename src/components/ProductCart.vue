<template>
  <section class="cart">
    <h2>Cart</h2>
    <div v-if="cart.length === 0" class="empty-cart">Cart is empty</div>
    <div v-else class="cart-items">
      <div v-for="item in cart" :key="item.id" class="cart-item">
        <img :src="item.image" :alt="item.name" class="cart-item-image" />
        <div class="cart-item-details">
          <h3>{{ item.name }}</h3>
          <p>{{ getItemPrice(item) }} UAH</p>
          <div class="cart-actions">
            <div class="quantity-controls">
              <span>Qty:</span>
              <div class="buttons">
                <a
                  @click="$emit('decrease-quantity', item)"
                  class="quantity-btn minus"
                >
                  -
                </a>
                <span>{{ item.quantity }}</span>
                <a
                  @click="$emit('increase-qty', item)"
                  class="quantity-btn plus"
                >
                  +
                </a>
              </div>
            </div>
            <a @click="$emit('remove-from-cart', item)" class="remove-btn"></a>
          </div>
        </div>
      </div>
      <div class="cart-total">
        <strong>Order Total: {{ totalAmount }} UAH</strong>
        <button @click="$emit('clear-cart')" class="clear-btn">
          Clear the cart
        </button>
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed, defineProps, defineEmits } from "vue";

// Props cart
const props = defineProps({
  cart: {
    type: Array,
    required: true,
  },
});

defineEmits([
  "increase-qty",
  "decrease-quantity",
  "remove-from-cart",
  "clear-cart",
]);

// Calculating the price of one product
const getItemPrice = (item) => {
  const price = item.discountPrice || item.price;
  return price * item.quantity;
};

// Calculating the total amount, using props.cart
const totalAmount = computed(() => {
  return props.cart.reduce((total, item) => total + getItemPrice(item), 0);
});
</script>

<style lang="scss" scoped>
.cart {
  background: #f5f5f5;
  padding: 10px 20px 10px;
}

.cart-item {
  display: flex;
  margin-bottom: 20px;
  padding-bottom: 20px;
  border-bottom: 1px solid #ccc;
}

.cart-item-details {
  width: 100%;
  margin-left: 10px;

  h3 {
    line-height: normal;
    font-size: 14px;
    margin-bottom: 5px;
    font-weight: normal;
  }

  p {
    font-weight: bold;
    margin-bottom: 5px;
  }
}

.cart-item-image {
  width: 70px;
  object-fit: contain;
  object-position: top;
}

.cart-actions {
  display: flex;
  justify-content: space-between;
}

.quantity-controls {
  display: flex;
  align-items: center;

  .buttons {
    margin-left: 5px;
    border: 2px solid #ccc;
    height: 35px;
    display: flex;
    align-items: center;
    border-radius: 5px;

    span {
      width: 35px;
      text-align: center;
    }

    .quantity-btn {
      cursor: pointer;
      width: 35px;
      text-align: center;
      height: inherit;
      display: flex;
      align-items: center;
      justify-content: center;

      &.minus {
        border-right: 2px solid #ccc;
      }

      &.plus {
        border-left: 2px solid #ccc;
      }
    }
  }
}

.remove-btn {
  display: block;
  width: 30px;
  height: 30px;
  position: relative;
  cursor: pointer;

  &:after {
    content: "";
    position: absolute;
    background-size: contain;
    background-repeat: no-repeat;
    background-image: url("../assets/images/icon-delete.svg");
    top: 50%;
    transform: translateY(-50%);
    left: 0;
    height: 30px;
    width: 30px;
  }

  &:hover::after {
    filter: brightness(0) saturate(100%) invert(33%) sepia(83%) saturate(2307%)
      hue-rotate(197deg) brightness(97%) contrast(91%);
  }
}

.cart-total {
  display: flex;
  flex-direction: column;

  .clear-btn {
    padding: 0;
    border: none;
    background: transparent;
    text-align: left;
    color: #1979c3;
    margin: 5px 0px 5px;
    cursor: pointer;
    &:hover {
      text-decoration: underline;
    }
  }
}

.empty-cart {
  margin-bottom: 10px;
}

//Desktop styles
@media screen and (min-width: 1025px) {
  .cart {
    position: fixed;
    width: inherit;
    max-width: 330px;
    min-width: 330px;
    max-height: 80vh;
    overflow-y: auto;
    padding: 10px 20px 0;
  }

  .cart-total {
    position: sticky;
    bottom: 0;
    background: #f5f5f5;
    padding-bottom: 10px;
    padding-top: 10px;
  }
}
</style>
