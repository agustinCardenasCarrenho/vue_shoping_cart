<template>
  <div v-for="product in products" :key="product">
    <Product
      :name="product.name"
      :image="product.imgUrl"
      :price="product.price"
    />
    <select  @change="updateCantItemFromLocalStorage"  v-model="product.cant" v-bind:data_id="product.id">
      <option
        v-for="index in 10"
        :key="index"
        :selected="index == product.cant"
        >
        {{ index }}
      </option>
    </select>
    <button @click="removeItemFromLocalStorage(product.id)">X</button>
  </div>
</template>

<script>
import Product from "../components/Product.vue";
export default {
  components: { Product },
  data() {
    return {
      products: JSON.parse(localStorage.getItem("cart")),
    };
  },
  mounted(){

  },
  methods: {
    removeItemFromLocalStorage(productId) {
      const index = this.products.findIndex(
        (product) => product.id == productId
      );
      this.products.splice(index, 1);
      localStorage.setItem("cart", JSON.stringify(this.products));
    },
    updateCantItemFromLocalStorage(event) {
      const index = this.products.findIndex(
        (product) =>  event.currentTarget.getAttribute("data_id") == product.id
      );
      this.products[index].cant = event.target.value;
      localStorage.setItem("cart", JSON.stringify(this.products));
    },
  },
};
</script>

<style>
</style>