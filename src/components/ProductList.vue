<template>
  <transition-group
    name="fade"
    @before-enter="before"
    @enter="enter"
    @leave="leave"
  >
    <div
      class="row d-none mb-3 align-items-center mt-4"
      v-for="(item, index) in products"
      :key="item.id"
      :data-index="index"
    >
      <div class="row align-items-center" v-if="item.price <= Number(maximum)">
        <div class="col-1 m-auto">
          <button class="btn btn-info" @click="$emit('add', item)">+</button>
        </div>
        <div class="col-sm-4">
          <img
            :src="item.images[0] || 'https://via.placeholder.com/150'"
            :alt="item.title"
            class="img-thumbnail d-block"
          />
        </div>
        <div class="col">
          <h3 class="text-info">{{ item.title }}</h3>
          <p class="mb-0">{{ item.description }}</p>
          <div class="h5 float-right">
            <price-component :value="Number(item.price)"></price-component>
          </div>
        </div>
      </div>
    </div>
  </transition-group>
</template>

<script>
import PriceComponent from "./PriceComponent";

export default {
  name: "product-list",
  props: {
    products: {
      type: Array,
      required: true,
    },
    maximum: {
      type: Number,
      required: true,
    },
  },
  components: {
    PriceComponent,
  },
  computed: {
    showItem: function () {
      let max = this.maximum;
      return this.products.filter(function (item) {
        return item.price <= this.max;
      });
    },
  },
  methods: {
    before(el) {
      el.className = "d-none";
      el.style.opacity = 0;
    },
    enter(el) {
      const delay = el.dataset.index * 100;
      setTimeout(() => {
        el.className =
          "row d-flex mb-3 align-items-center mt-4 animate__animated animate__fadeInRight";
        el.style.opacity = 1;
      }, delay);
    },
    leave(el) {
      const delay = el.dataset.index * 100;
      setTimeout(() => {
        el.className =
          "row d-flex mb-3 align-items-center mt-4 animate__animated animate__fadeOutRight";
        el.style.opacity = 0;
        setTimeout(() => {
          el.remove(); // Menghapus elemen setelah animasi selesai
        }, 500);
      }, delay);
    },
  },
};
</script>
