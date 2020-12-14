<template>
  <div>
    <div class="foods">
      <Header />
    </div>
    <div class="container mt-5">
      <h2>Food <strong>Products</strong></h2>

      <div class="mb-5 mt-4">
        <input
          v-model="search"
          type="text"
          class="form-control"
          id="search"
          placeholder="Cari Makanan Favoritmu"
          @keyup="searchProduct"
        />
      </div>

      <div class="row mt-3 mt-3 mb-5">
        <div
          class="col-md-4 mb-4"
          v-for="product in products"
          :key="product.id"
        >
          <CardProduct :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";

export default {
  name: "Food",
  components: {
    Header,
    CardProduct,
  },
  data() {
    return {
      products: [],
      search: '',
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
    searchProduct() {
    axios
      .get("http://localhost:3000/products?q=" + this.search)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  }
  },
  mounted() {
    axios
      .get("http://localhost:3000/products")
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
</style>