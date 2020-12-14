<template>
  <div class="home">
    <Header />
    <div class="container">
      <Hero />

      <div class="row mt-4">
        <div class="col">
          <h2>Best Our <strong>Foods</strong></h2>
        </div>
        <div class="col">
          <router-link to="/food" class="link-semua float-right"
            >Semuanya <b-icon-arrow-right></b-icon-arrow-right
          ></router-link>
        </div>
      </div>

      <div class="row mt-3 mb-5">
        <div class="col-md-4 mt-d" v-for="product in products" :key="product.id">
          <CardProduct :product="product"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import Hero from "@/components/Hero.vue";
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Header,
    Hero,
    CardProduct,
  },
  data() {
    return {
      products: [],
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/best-products")
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error))
  },
};
</script>
