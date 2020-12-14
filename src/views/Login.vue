<template>
  <div>
    <div class="login">
      <Header />
    </div>
    <div class="container">
      <div id="card">
        <div id="card-content">
          <div id="card-title">
            <h1 style="color: #4eb883">RESTO LINOV</h1>
            <hr />
          </div>
          <form @submit.prevent="loginSubmit">
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
              Masuk
            </button>

            <hr />
            <p>
              Belum Punya Akun? <router-link to="/register">Daftar</router-link>
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
  name: "Login",
  components: {
    Header,
  },
  data() {
    return {
      username: "",
      password: "",
    };
  },
  methods: {
    loginSubmit() {
      axios
        .get(`http://localhost:3000/users?username=${this.username}`)
        .then((respone) => {
          if (respone.data.length <= 0) {
            this.$toast.error("Usernama tidak terdaftar", {
              type: "error",
              position: "top",
              duration: 3000,
              dismissible: true,
            });
            return;
          }
          if (respone.data[0].password === this.password) {
            return true;
          }
          else
          {
            return false;
          }
        })
        .then((respone) => {
          if (respone) {  
            this.$store.commit("setAuthenticated", true);
            this.$router.push("/");
          } else if (respone === false) {
            this.$toast.error("Password Anda Salah", {
              type: "error",
              position: "top",
              duration: 3000,
              dismissible: true,
            });
          }
        });
    },
  },
};
</script>

<style>
</style>