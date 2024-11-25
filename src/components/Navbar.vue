<template>
  <nav class="navbar navbar-light bg-light">
    <div class="navbar-text ml-auto d-flex align-items-center">
      <button class="btn btn-sm btn-outline-success" @click="$emit('toggle')">
        <font-awesome-icon icon="dollar-sign"></font-awesome-icon>
      </button>
      <div class="dropdown ml-2" v-if="cart.length > 0">
        <button
          class="btn btn-success btn-sm dropdown-toggle"
          id="dropdnCart"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false"
        >
          <span class="badge badge-fill badge-success">{{ cartQty }}</span>
          <font-awesome-icon icon="shopping-cart"></font-awesome-icon>
          {{ cartTotal | currencyFormat }}
        </button>
        <div
          class="dropdown-menu dropdown-menu-right"
          aria-labelledby="dropdownCart"
        >
          <div v-for="(item, index) in groupedCart" :key="item.product.id">
            <div
              class="dropdown-item-text text-nowrap text-right d-flex justify-content-end align-items-center"
            >
              <div class="d-block wrap-item-img">
                <span
                  class="badge badge-pill badge-warning align-text-top mr-1 qty-product"
                >
                  {{ item.qty }}
                </span>
                <img
                  :src="item.product.images"
                  :alt="item.product.title"
                  class="d-block img-cart rounded-sm"
                />
              </div>
              {{ item.product.title }}
              <b class="mr-1">{{
                (item.product.price * item.qty) | currencyFormat
              }}</b>
              <a
                href="#"
                class="badge badge-danger text-white"
                @click.stop="$emit('delete', item.product.id)"
                >-</a
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>
<script>
import PriceComponent from "./PriceComponent.vue";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
export default {
  name: "navbar",
  components: {
    PriceComponent,
    FontAwesomeIcon,
  },
  props: ["cart", "cartQty", "cartTotal"],
  computed: {
    groupedCart() {
      return this.cart.reduce((acc, current) => {
        const existingItem = acc.find(
          (item) => item.product.id === current.product.id
        );
        if (existingItem) {
          existingItem.qty += current.qty;
        } else {
          acc.push({ ...current });
        }
        return acc;
      }, []);
    },
  },
  filters: {
    currencyFormat: function (value) {
      return "Rp " + Number.parseFloat(value).toFixed(2);
    },
  },
};
</script>

<style>
.img-cart {
  width: 60px;
  height: 60px;
  object-fit: cover;
}
.qty-product {
  position: absolute;
  top: -5px;
  right: -10px;
}
.wrap-item-img {
  position: relative;
}
</style>
