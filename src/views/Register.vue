<template>
  <div>
    <div class="regsiter">
      <Header />
    </div>
    <div class="container">
      <div id="card">
        <div id="card-content">
          <div id="card-title">
            <h1 style="color: #4eb883">RESTO LINOV</h1>
            <p>Silahakan isi Form Pendaftaran</p>
            <hr />
          </div>
          <form @submit.prevent="registerSubmit">
            <div class="form-group">
              <input
                type="text"
                class="form-control form-control-user"
                v-model="nama"
                placeholder="Nama Lengkap"
              />
            </div>
            <div class="form-group">
              <input
                type="text"
                class="form-control form-control-user"
                v-model="username"
                placeholder="Username"
              />
            </div>
            <div class="form-group">
              <input
                type="email"
                class="form-control form-control-user"
                v-model="email"
                placeholder="E-mail"
              />
            </div>
            <div class="form-group">
              <input
                type="password"
                class="form-control form-control-user"
                id="password"
                v-model="password"
                placeholder="Password"
              />
            </div>
            <button
              type="submit"
              name="login"
              class="btn btn-success btn-user btn-block"
            >
              Daftar
            </button>

            <hr />
            <p>
              Sudah Punya Akun? <router-link to="/login">Login</router-link>
            </p>
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
  name: "Register",
  components: {
    Header,
  },
  data() {
    return {
      nama: "",
      username: "",
      email: "",
      password: "",
    };
  },
  methods: {
    registerSubmit() {
      const userRegis = {
        nama: this.nama,
        username: this.username,
        email: this.email,
        password: this.password,
      };
      if (this.nama && this.username && this.email && this.password) {
        axios.post("http://localhost:3000/users", userRegis).then(() => {
          this.$router.push({ path: "/login" });
          this.$toast.success("Pendaftaran Berhasil", {
            type: "success",
            position: "top",
            duration: 3000,
            dismissible: true,
          });
        });
      } else {
        this.$toast.success("Harap Isi Semua Field", {
          type: "error",
          position: "top",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
};
</script>

<style>
</style>