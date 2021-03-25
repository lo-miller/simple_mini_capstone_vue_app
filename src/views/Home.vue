<template>
<div class="home">
  <h1>{{ message }}</h1>
  <br>
   <button v-on:click="indexProducts">Reload Products</button>
  <br>
  <button v-on:click="createProduct">Create a New Product</button>
  <br>
  <hr>

  <div v-for="product in products" v-bind:key="product.name">
    {{product.name}}
    <br>
    <img v-bind:src="product.image_url">
    <br>
    <button v-on:click="showProduct(product)">More Info</button>
    <hr>
  </div>

  <dialog id="product-details">
    <form method="dialog">
    <h3>Product Information</h3>
    <p>Product name: <input type="text" v-model="currentProduct.name"/>
    <!-- {{currentProduct.name}}</p> -->
    <p>Description: {{currentProduct.description}}</p>
    <p>Price: {{currentProduct.price}}</p>
    <img v-bind:src="currentProduct.image_url">
    </form>
    <button>Update Product</button>
    <!-- <button>Close</button> -->
  </dialog>

</div>
</template>
<style>
img {
  width: 250px;
}
</style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to the Products page!",
      products: [],
      name: "",
      description: "",
      image_url: "",
      price: "",
      newProduct: [],
      currentProduct: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/api/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProduct: function () {
      axios
        .post("http://localhost:3000/api/products", {
          name: "spiral notebook",
          description: "spiral-bound, lined notebook",
          price: 4,
          image_url:
            "https://decomposition.com/wp-content/uploads/Everglades_FullRes-3.jpg",
        })
        .then((response) => {
          console.log(response.data);
          this.products.push(response.data);
        });
    },
    showProduct: function (theProduct) {
      console.log(theProduct);
      this.currentProduct = theProduct;
      document.querySelector("#product-details").showModal();
    },
    // updateProduct: function () {
    //   // axios
    //   // .patch()
    // },
  },
};
</script>