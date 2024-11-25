<template>
  <div class="container p-3" v-if="cart.length">
    <h1>Checkout</h1>
    <table class="table table-hover">
      <caption class="text-right h3">
        <b>Total : </b>
        <price-component
          class="d-block text-success font-weight-light"
          :value="Number(cartTotal)"
        ></price-component>
      </caption>
      <thead>
        <tr>
          <th scope="col">Actions</th>
          <th scope="col">Item</th>
          <th scope="col" class="text-center">Qty</th>
          <th scope="col" class="text-right">Price</th>
          <th scope="col" class="text-right">Sub-total</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in groupedCart" :key="item.product.id">
          <td class="text-center">
            <div class="btn-group">
              <button @click="$emit('add', item.product)" class="btn btn-info">
                +
              </button>
              <button
                @click="$emit('delete', item.product.id)"
                class="btn btn-outline-info"
              >
                -
              </button>
            </div>
          </td>
          <th scope="row">{{ item.product.title }}</th>
          <th class="text-center">{{ item.qty }}</th>
          <th class="text-right">{{ Number(item.product.price) }}</th>
          <th class="text-right">
            {{ Number(item.product.price * item.qty).toFixed(2) }}
          </th>
        </tr>
      </tbody>
    </table>
    <router-link class="btn btn-sm btn-outline-info text-info" to="/"
      >Back to Shop</router-link
    >
  </div>
</template>
<script>
import PriceComponent from "./PriceComponent.vue";
export default {
  name: "checkout",
  props: ["cart", "cartTotal"],
  components: {
    PriceComponent,
  },
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
};
</script>
