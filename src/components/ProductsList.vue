<template>
  <div class="row">
    <div class="column" v-for="product in products" :key="product.name">
      <Product
        :name="product.name"
        :description="product.description"
        :image="product.imgUrl"
        :price="product.price"
      />
      <button @click="addProductToCart(product.id)">add</button>
    </div>
  </div>
  <div class="overlay closed" ref="modalCart">
    <button
      style="
        right: 0;
        margin-right : 20px;
        position: fixed;
        background-color: transparent;
        color: white;"
        @click="closeModalCart"
    >
      x
    </button>
    <div  v-for="x in fakeCart" :key="x.name">
      <Product :name="x.name" :image="x.imgUrl" :price="x.price" />
    </div>
  </div>
</template>

<script>
import data from "../data/productsList.json";
import Product from "./Product.vue";

let productArrayLocalStorage = JSON.parse(localStorage.getItem("cart"));

export default {
  components: {
    Product,
  },
  data: function () {
    return {
      products: [],
      fakeCart: []
    };
  },
  mounted() {
    data.products.map(({ id, name, price, imgUrl, description }) => {
      this.products = [
        ...this.products,
        { id, name, price, imgUrl, description },
      ];
    }); 
    productArrayLocalStorage == null
      ? localStorage.setItem("cart", JSON.stringify([]))
      : JSON.parse(localStorage.getItem("cart"));
  },
  methods: {
    addProductToCart(productId) {
      const product = this.products.find((product) => product.id === productId);
      productArrayLocalStorage = JSON.parse(localStorage.getItem("cart"));
      if (
        productArrayLocalStorage.find((p) => p.id == product.id) == undefined
      ) {
        product.cant = 1;
        productArrayLocalStorage.push(product);
      } else {
        const index = productArrayLocalStorage.findIndex(
          (product) => product.id == productId
        );
        productArrayLocalStorage[index].cant++;
      }
      localStorage.setItem("cart", JSON.stringify(productArrayLocalStorage));
      this.fakeCart = JSON.parse(localStorage.getItem('cart'));
      this.openModelCart();
    },
    openModelCart(){
      this.$refs.modalCart.classList.remove("closed");
      document.body.style = "overflow: hidden;" 
    },
    closeModalCart(event){
      event.target.parentElement.classList.add("closed");
       document.body.style = "overflow: "; 
    },
  },
};
</script>

<style>
.row {
  display: grid;
  grid-template-columns: auto auto auto;
  padding: 10px;
}

.column {
  padding: 20px;
  font-size: 30px;
  text-align: center;
}

.overlay {
  position: fixed;
   overflow-x:hidden;
  width: 40%;
  height: 100%;
  top: 0;
  right: 0;
  bottom: 0;
  background-color: rgb(121, 119, 119);
  z-index: 2;
}
.closed{
  display: none;
}

</style>