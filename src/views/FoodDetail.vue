<template>
  <div class="foodDetail">
    <Header />

    <div class="container">
      <div class="row">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb mt-4 mb-5">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/food" class="text-dark">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Food Order
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6">
          <img
            :src="'../assets/img/' + product.gambar"
            alt=""
            class="img-fluid img"
          />
        </div>
        <div class="col-md-6 mt-2">
          <h2>{{ product.nama }}</h2>
          <hr />
          <h4>Rp. {{ product.harga }}</h4>
          <form class="form-group mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlahpesanan">Jumlah Pesanan</label>
              <input
                type="number"
                class="form-control"
                min="1"
                required
                v-model="pesanan.jumlah_pesanan"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                cols="30"
                rows="2"
                class="form-control"
                v-model="pesanan.keterangan"
                placeholder="NB: Jika lebih dari 1.. 2.."
              ></textarea>
            </div>
            <button type="submit" class="btn btn-success" @click="pesan">
              <b-icon-cart></b-icon-cart> Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header.vue";
import axios from "axios";

export default {
  name: "FoodDetail",
  components: {
    Header,
  },
  data() {
    return {
      product: {},
      pesanan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
    pesan() {
      if (this.pesanan.jumlah_pesanan) {
        this.pesanan.products = this.product;
        axios
          .post("http://localhost:3000/keranjangs", this.pesanan)
          .then(() => {
            this.$router.push({ path: "/keranjang" });
            this.$toast.success("Berhasil Masuk Keranjang", {
              type: "success",
              position: "top",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err));
      } else {
        this.$toast.error("Jumlah Pesanan Harus Diisi!!!", {
          type: "error",
          position: "top",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    if (this.$store.state.isAuthenticated) {
      axios
        .get("http://localhost:3000/products/" + this.$route.params.id)
        .then((response) => this.setProduct(response.data))
        .catch((error) => console.log(error));
    } else {
      this.$router.push("/login");
      this.$toast.error("Silahkan Login Kembali", {
        type: "error",
        position: "top",
        duration: 3000,
        dismissible: true,
      });
    }
  },
};
</script>

<style>
</style>