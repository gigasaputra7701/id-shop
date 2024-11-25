<template>
  <div>
    <navbar
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      @toggle-slide="toggleSlider"
      @delete-item="deleteItem"
    ></navbar>
    <div class="container mt-5 p-4">
      <h1>IdShop</h1>
      <price-slider
        :sliderStatus="sliderStatus"
        :maximum.sync="maximum"
      ></price-slider>
      <product-list
        :products="products"
        :maximum="maximum"
        @add-item="addItem"
      ></product-list>
    </div>
  </div>
</template>
<script>
import ProductList from "./ProductList.vue";
import PriceSlider from "./PriceSlider.vue";
import Navbar from "./Navbar.vue";

export default {
  name: "products",
  props: [
    "products",
    "maximum",
    "cart",
    "cartQty",
    "cartTotal",
    "sliderStatus",
    "sliderState",
  ],
  components: {
    ProductList,
    PriceSlider,
    Navbar,
  },
  methods: {
    toggleSlider: function () {
      this.$emit("toggle");
    },
    addItem: function (item) {
      this.$emit("add", item);
    },
    deleteItem: function (productId) {
      this.$emit("delete", productId);
    },
  },
};
</script>
