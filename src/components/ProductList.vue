<template>
  <div class="products">
    <h2>Product List</h2>
    <table>
      <thead>
        <tr>
          <th class="col item">Item</th>
          <th class="col price">Price</th>
          <th class="col qty">Qty</th>
          <th class="col subtotal">Subtotal</th>
          <th></th>
        </tr>
      </thead>
      <tbody v-for="product in products" :key="product.id">
        <tr class="item-info">
          <td class="col item">
            <div class="item-inner">
              <img
                :src="product.image"
                :alt="product.name"
                class="product-image"
              />
              <h3>{{ product.name }}</h3>
            </div>
          </td>
          <td class="col price" data-th="Price">
            <p>
              <span v-if="product.discountPrice" class="discount-price">
                {{ product.discountPrice }} UAH
              </span>
              <span :class="{ 'original-price': product.discountPrice }">
                {{ product.price }} UAH
              </span>
            </p>
          </td>
          <td class="col qty" data-th="Qty">
            <input
              type="number"
              v-model.number="quantities[product.id]"
              min="1"
              placeholder="1"
              class="qty-input"
            />
          </td>
          <td class="col subtotal" data-th="Subtotal">
            {{ calculateSubtotal(product) }} UAH
          </td>
          <td class="col button">
            <button
              @click="addToCart(product)"
              :disabled="isAdding[product.id]"
              :class="{ 'adding-state': isAdding[product.id] }"
              class="add-btn"
            >
              <span class="btn-text">
                {{
                  isAdding[product.id] === "adding"
                    ? "Adding..."
                    : isAdding[product.id] === "added"
                    ? "Added"
                    : "Add to cart"
                }}
              </span>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script setup>
import { ref, defineEmits } from "vue";
import productImage1 from "@/assets/images/products/art-deco-walnut-drinks-trolley-c-1930.webp";
import productImage2 from "@/assets/images/products/japanese-mizuya-tansu-chest-of-drawers.webp";
import productImage3 from "@/assets/images/products/antique-pembroke-mahogany-sofa-table.webp";
import productImage4 from "@/assets/images/products/finnish-art-deco-coffee-table-with-karelian-birch.webp";
import productImage5 from "@/assets/images/products/art-deco-bentwood-stool-in-walnut.webp";
import productImage6 from "@/assets/images/products/danish-midcentury-teak-coffee-table.webp";
import productImage7 from "@/assets/images/products/retro-orange-egg-pod-space-age-chair.webp";

const products = ref([
  {
    id: 1,
    name: "Art Deco Walnut Drinks Trolley c.1930",
    price: 40950,
    discountPrice: 30000,
    image: productImage1,
  },
  {
    id: 2,
    name: "Early 20th Century Japanese Mizuya Tansu Chest of Drawers in Chestnut",
    price: 60650,
    image: productImage2,
  },
  {
    id: 3,
    name: "Antique Pembroke Mahogany Sofa Table",
    price: 50550,
    discountPrice: 40000,
    image: productImage3,
  },
  {
    id: 4,
    name: "Finnish Art Deco Coffee Table with Karelian Birch Top c.1935",
    price: 5595,
    image: productImage4,
  },
  {
    id: 5,
    name: "Art Deco Bentwood Stool in Walnut by Hille, Re-upholstered in Art Deco Fabric",
    price: 9950,
    image: productImage5,
  },
  {
    id: 6,
    name: "Danish Midcentury Teak Coffee Table by Arne Hovmand Olsen c.1965",
    price: 20000,
    image: productImage6,
  },
  {
    id: 7,
    name: "Retro Orange Egg Pod Space Age Chair",
    price: 30250,
    image: productImage7,
  },
]);

const quantities = ref({});
const isAdding = ref({});

const calculateSubtotal = (product) => {
  const qty = quantities.value[product.id] || 1;
  const price = product.discountPrice || product.price; // Take the discounted price, if there is one.
  return price * qty;
};

// Function for add a product to the cart
const addToCart = (product) => {
  const qty = quantities.value[product.id] || 1; // If the quantity is not specified, the default is 1.
  if (qty > 0) {
    isAdding.value[product.id] = "adding";
    setTimeout(() => {
      isAdding.value[product.id] = "added";
      setTimeout(() => {
        emit("add-to-cart", { ...product, quantity: qty });
        quantities.value[product.id] = null;
        isAdding.value[product.id] = false;
      }, 1000);
    }, 1000);
  }
};

const emit = defineEmits(["add-to-cart"]);
</script>

<style lang="scss" scoped>
table {
  border-collapse: collapse;
  th,
  td {
    vertical-align: top;
  }
}
.col {
  &.item {
    text-align: left;
  }
}

tbody {
  .col {
    &.subtotal {
      white-space: nowrap;
      font-weight: bold;
    }
    &.price {
      p {
        display: flex;
        flex-direction: column;
        white-space: nowrap;
        font-weight: bold;

        .original-price {
          position: relative;
          color: #ccc;

          &:after {
            content: "";
            width: 100%;
            height: 1px;
            background: red;
            position: absolute;
            bottom: 13px;
            left: 0;
            transform: rotate(170deg);
          }
        }
      }
    }
  }
}

.item-inner {
  display: flex;

  h3 {
    margin-left: 10px;
    line-height: normal;
  }
}
.qty-input {
  height: 36px;
  width: 60px;
  text-align: center;
  border: 2px solid #ccc;
  border-radius: 5px;
}

.add-btn {
  background: #1979c3;
  border: 1px solid #1979c3;
  color: #fff;
  font-weight: 600;
  border-radius: 3px;
  padding: 5px 15px;
  white-space: nowrap;
  cursor: pointer;
  transition: background-color 0.3s ease, opacity 0.3s ease;
  min-width: 120px;

  &:hover {
    background: #006bb4;
    border: 1px solid #006bb4;
  }

  &.adding-state {
    opacity: 0.7;
    cursor: not-allowed;
  }
}

//Mobile styles
@media screen and (max-width: 1024px) {
  table {
    thead {
      .col {
        display: none;
      }
    }
    tbody {
      border-bottom: 1px solid #ccc;
    }
  }
  .item-info {
    .col {
      display: inline-block;

      &.item {
        padding: 10px 0 10px;
        width: 100%;
      }

      &.price {
        p,
        .discount-price {
          display: block;
        }

        p {
          .original-price {
            position: relative;
            color: #ccc;

            &:after {
              bottom: 9px;
            }
          }
        }
      }

      &.price,
      &.qty,
      &.subtotal,
      &.button {
        width: 25%;
        text-align: center;
        padding-bottom: 10px;
      }
      &.price,
      &.qty,
      &.subtotal {
        &:before {
          content: attr(data-th);
          display: block;
          padding-bottom: 5px;
          font-weight: normal;
        }
      }
    }
  }
  .product-image {
    width: 150px;
  }
}

@media screen and (max-width: 430px) {
  .item-info {
    .col {
      &.price,
      &.qty,
      &.subtotal {
        width: 33%;
      }

      &.button {
        width: 100%;
      }
    }
  }
}

//Desktop styles
@media screen and (min-width: 1025px) {
  .product-image {
    max-width: 100px;
    object-fit: contain;
    object-position: top;
  }

  h2 {
    padding-top: 10px;
  }

  thead {
    th {
      border-bottom: 1px solid #ccc;
      padding-top: 24px;
      padding-bottom: 10px;
    }
  }

  thead,
  tbody {
    .col {
      padding-top: 10px;
      &.price,
      &.qty,
      &.subtotal {
        text-align: right;
        padding-right: 15px;
        padding-left: 15px;
      }
    }
  }
  tbody {
    td {
      border-top: 1px solid #ccc;
      padding-bottom: 10px;
    }
  }
}
@media screen and (min-width: 1210px) {
  .product-image {
    max-width: 165px;
  }
}
</style>
