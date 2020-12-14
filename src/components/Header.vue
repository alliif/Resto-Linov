<template>
  <nav class="navbar navbar-expand-lg navbar-light">
    <div class="container pt-3">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Resto Linov</a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav mr-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link class="nav-link" to="/">Home</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/food">Foods</router-link>
            </li>
          </ul>

          <ul
            class="navbar-nav ml-auto mb-2 mb-lg-0"
            v-if="this.$store.state.isAuthenticated"
          >
            <li class="nav-item">
              <router-link class="nav-link" to="/keranjang">
                <b-icon-bag></b-icon-bag>
                <!-- <span
                  v-if="this.$store.state.keranjangCount !== 0"
                  class="badge badge-success ml-1"
                  >{{ this.$store.state.keranjangCount }}</span> -->
                <span class="badge badge-success ml-1">{{
                  updateKeranjang
                    ? updateKeranjang.length
                    : this.keranjangs.length
                }}</span>
              </router-link>
            </li>
            <li class="nav-item">
              <a class="nav-link" style="cursor: pointer" @click="logout">
                Logout
              </a>
            </li>
          </ul>

          <ul class="navbar-nav ml-auto mb-2 mb-lg-0" v-else>
            <li class="nav-item">
              <router-link class="nav-link" to="/login"> Login </router-link>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
import axios from "axios";

export default {
  name: "Header",
  data() {
    return {
      keranjangs: [],
    };
  },
  props: ["updateKeranjang"],
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    logout() {
      this.$store.commit("setAuthenticated", false);
      this.$toast.success("Anda Telah Logout", {
        type: "success",
        position: "top",
        duration: 3000,
        dismissible: true,
      });
      this.$router.push("/");
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setKeranjangs(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
</style>