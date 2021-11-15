<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs" />
    <div class="container">
      <!-- Breadcrumb -->
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <th>{{ index + 1 }}</th>
                  <td>
                    <img
                      :src="`assets/images/${keranjang.products.gambar}`"
                      alt="product"
                      class="img-fluid shadow"
                      width="100"
                    />
                  </td>
                  <td>
                    <strong>{{ keranjang.products.nama }}</strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                  </td>
                  <td align="center">{{ keranjang.jumlah_pemesanan }}</td>
                  <td align="right">Rp. {{ keranjang.products.harga }}</td>
                  <td align="right">
                    <strong
                      >Rp.
                      {{
                        keranjang.products.harga * keranjang.jumlah_pemesanan
                      }}
                    </strong>
                  </td>
                  <td align="center">
                    <button
                      class="btn btn-sm btn-danger"
                      @click="hapusMenu(keranjang.id)"
                    >
                      <b-icon-trash></b-icon-trash>
                    </button>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" align="right">
                    <strong> Total Bayar: </strong>
                  </td>
                  <td align="right">
                    <strong> Rp. {{ totalHarga }} </strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar.vue";
import axios from "axios";
export default {
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    hapusMenu(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          // handle success
          this.$toast.error("Menu berhasil dihapus...", {
            type: "error",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });

          axios
            .get("http://localhost:3000/keranjangs")
            .then((response) => {
              // handle success
              this.setKeranjangs(response.data);
            })
            .catch((error) => {
              // handle error
              console.log("Failed: ", error);
            })
            .then(function () {
              // always executed
            });
        })
        .catch((error) => {
          // handle error
          console.log("Failed: ", error);
        })
        .then(function () {
          // always executed
        });
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => {
        // handle success
        this.setKeranjangs(response.data);
      })
      .catch((error) => {
        // handle error
        console.log("Failed: ", error);
      })
      .then(function () {
        // always executed
      });
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return data.products.harga * data.jumlah_pemesanan + items;
      }, 0);
    },
  },
};
</script>

<style></style>
