<template>
  <div class="card shadow card-product">
    <img
      :src="'assets/img/' + product.gambar"
      class="card-img-top"
      alt="image"
    />
    <div class="card-body">
      <h5 class="card-title">{{ product.nama }}</h5>
      <p class="card-text">
        Harga : Rp. {{product.harga}}
      </p>
      <router-link v-if="this.$store.state.isAuthenticated" :to="'/food/'+product.id" @click="addKeranjang" class="btn btn-success">Pesan</router-link>
      <router-link v-else :to="'/login'" class="btn btn-success">Pesan</router-link>
    </div>
  </div>
</template>

<script>
import {mapActions} from "vuex";

export default {
  name: "CardProduct",
  props: ["product"],

  methods:
  {
    ...mapActions(["addKeranjang"])
  }
};
</script>

<style>
</style>