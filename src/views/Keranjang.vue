<template>
  <div>
    <div class="keranjang">
      <Header :updateKeranjang="keranjang" />
    </div>
    <div class="container mt-5">
      <h2>Daftar <strong>Pesanan</strong></h2>
      <div class="row">
        <div class="col">
          <table class="table table-striped mt-4">
            <thead>
              <tr>
                <th scope="col">No.</th>
                <th scope="col">Nama Pesanan</th>
                <th scope="col">Harga</th>
                <th scope="col">Jumlah</th>
                <th scope="col">Keterangan</th>
                <th scope="col">Total Harga</th>
                <th scope="col">Aksi</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(cart, index) in keranjang" :key="cart.id">
                <th>{{ index + 1 }}</th>
                <td>{{ cart.products.nama }}</td>
                <td>Rp. {{ cart.products.harga }}</td>
                <td>{{ cart.jumlah_pesanan }}</td>
                <td>{{ cart.keterangan ? cart.keterangan : "-" }}</td>
                <td>Rp. {{ cart.jumlah_pesanan * cart.products.harga }}</td>
                <td align="left" class="text-danger mr-auto">
                  <div
                    class="btn-group"
                    role="group"
                    aria-label="Basic mixed styles example"
                  >
                    <button
                      @click="hapusPesanan(cart.id)"
                      type="button"
                      class="btn btn-danger btn-sm"
                    >
                      Hapus
                    </button>
                    <button
                      @click="getPesananById(cart.id)"
                      type="button"
                      class="btn btn-success btn-sm"
                      data-bs-toggle="modal"
                      data-bs-target="#keranjang"
                    >
                      Ubah
                    </button>
                  </div>
                </td>
              </tr>

              <tr>
                <td colspan="5" align="right"><strong>Total Harga</strong></td>
                <td>
                  <strong>Rp. {{ totalHarga }}</strong>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <div class="row justify-content-end">
        <div class="col-md-4">
          <form class="mt-3" v-on:submit.prevent>
            <div class="form-group">
              <label for="nama">Nama Pemesan</label>
              <input type="text" class="form-control" v-model="checkout.nama" />
            </div>
            <div class="form-group">
              <label for="nama">Nomor Meja</label>
              <input
                type="number"
                class="form-control"
                v-model="checkout.noMeja"
              />
            </div>
            <button
              type="submit"
              class="btn btn-success btn-sm float-right"
              @click="Checkout"
            >
              Checkout
            </button>
          </form>
        </div>
      </div>

      <div
        class="modal fade"
        id="exampleModal"
        tabindex="-1"
        aria-labelledby="exampleModalLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">
                Form Update Pesanan
              </h5>
            </div>
            <div class="modal-body">
              <form v-on:submit.prevent>
                <div class="mb-3 form-group">
                  <label for="jumlah" class="form-label">Jumlah</label>
                  <input
                    type="number"
                    class="form-control"
                    min="1"
                    id="jumlah"
                    v-model="update.jumlah_pemesanan"
                  />
                </div>
                <button
                  type="submit"
                  @click="updateKeranjang"
                  class="btn btn-success float-end"
                >
                  Update
                </button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header.vue";
import axios from "axios";

export default {
  name: "Keranjang",
  components: {
    Header,
  },
  data() {
    return {
      keranjang: {},
      checkout: {},
      update: {},
    };
  },
  methods: {
    setKeranjang(data) {
      this.keranjang = data;
    },
    setUpdate(data) {
      this.update = data;
    },
    hapusPesanan(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.$toast.error("Data Pesanan Berhasil Dihapus!!!", {
            type: "error",
            position: "top",
            duration: 3000,
            dismissible: true,
          });

          // Update dan Reload
          axios
            .get("http://localhost:3000/keranjangs")
            .then((response) => this.setKeranjang(response.data))
            .catch((error) => console.log(error));
        })
        .catch((error) => console.log(error));
    },
    Checkout() {
      if (this.checkout.nama && this.checkout.noMeja) {
        this.checkout.keranjang = this.keranjang;
        axios
          .post("http://localhost:3000/pesanans", this.checkout)
          .then(() => {
            this.keranjang.map(function (item) {
              return axios
                .delete("http://localhost:3000/keranjangs/" + item.id)
                .catch((error) => console.log(error));
            });
            this.$router.push({ path: "/" });
            this.$toast.success("Pesanan Berhasil", {
              type: "success",
              position: "top",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err));
      } else {
        this.$toast.error("Nama Pemesan dan Nomor Meja Harus Diisi!!!", {
          type: "error",
          position: "top",
          duration: 3000,
          dismissible: true,
        });
      }
    },
    getPesananById(id) {
      const updateData = this.keranjang;
      for (var i = 0; i < updateData.length; i++) {
        if (updateData[i].id === id) {
          this.update = {
            id: id,
            jumlah_pemesanan: updateData[i].jumlah_pemesanan,
          };
        }
      }
    },
    updateKeranjang() {
      axios
        .patch(`http://localhost:3000/keranjangs/${this.update.id}`, {
          jumlah_pemesanan: this.update.jumlah_pemesanan,
        })
        .then(() => {
          this.$toast.success({
            message: "Update Data Berhasil",
            type: "success",
            position: "top",
            duration: 3000,
          });
        })
        .finally(() => {
          document.getElementById("keranjang").click("hide");
          this.update = {};
        });
    },
  },
  mounted() {
    if (this.$store.state.isAuthenticated) {
      axios
        .get("http://localhost:3000/keranjangs")
        .then((response) => this.setKeranjang(response.data))
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
  computed: {
    totalHarga() {
      return this.keranjang.reduce(function (item, data) {
        return item + data.products.harga * data.jumlah_pesanan;
      }, 0);
    },
  },
};
</script>

<style>
</style>