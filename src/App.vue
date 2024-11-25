<template>
  <div id="app">
    <router-view
      :cart="cart"
      :cartQty="cartQty"
      :cartTotal="cartTotal"
      :maximum.sync="maximum"
      :products="products"
      :sliderStatus="sliderStatus"
      @toggle="toggleSliderStatus"
      @add="addItem"
      @delete="deleteItem"
    >
    </router-view>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      maximum: 50,
      products: [],
      cart: [],
      sliderStatus: false,
    };
  },
  mounted() {
    fetch("https://dummyjson.com/products?limit=10")
      .then((res) => res.json())
      .then((data) => {
        this.products = data.products; // Properti 'products' dalam respons
      })
      .catch((error) => {
        console.error("Terjadi kesalahan:", error);
      });
  },

  computed: {
    cartTotal: function () {
      let sum = 0;
      for (let key in this.cart) {
        sum = sum + this.cart[key].product.price * this.cart[key].qty;
      }
      return sum;
    },
    cartQty: function () {
      let qty = 0;
      for (let key in this.cart) {
        qty = qty + this.cart[key].qty;
      }
      return qty;
    },
  },

  methods: {
    toggleSliderStatus: function () {
      this.sliderStatus = !this.sliderStatus;
    },
    addItem: function (product) {
      let productIndex;
      let productExist = this.cart.filter(function (item, index) {
        if (item.product.id == Number(product.id)) {
          productIndex = index;
        } else {
          return false;
        }
      });
      if (productExist.length) {
        this.cart[productIndex].qty++;
      } else {
        this.cart.push({ product: product, qty: 1 });
      }
    },
    // Menghapus item dari keranjang
    deleteItem: function (productId) {
      const productIndex = this.cart.findIndex(
        (item) => item.product.id === productId
      );
      if (productIndex !== -1) {
        if (this.cart[productIndex].qty > 1) {
          // Jika qty lebih dari 1, kurangi qty
          this.cart[productIndex].qty--;
        } else {
          // Jika qty hanya 1, hapus item sepenuhnya
          this.cart.splice(productIndex, 1);
        }
      }
    },
  },
};
</script>
<style>
* {
  padding: 0;
  margin: 0;
}
</style>
