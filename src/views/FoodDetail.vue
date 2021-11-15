<template>
  <div class="food-detail">
    <Navbar />
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
                Food Detail
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <div class="row mt-4">
        <div class="col-md-6">
          <img
            :src="`assets/images/${product.gambar}`"
            alt="images-detail"
            class="img-fluid shadow"
          />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
          </h2>
          <hr />
          <h4>
            Harga : <strong>Rp. {{ product.harga }}</strong>
          </h4>
          <form class="mt-3" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pemesanan">Jumlah Pesanan</label>
              <input
                type="number"
                class="form-control"
                placeholder="Masukkan jumlah pesanan"
                v-model="pesanan.jumlah_pemesanan"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                name="keterangan"
                id=""
                cols="30"
                rows="10"
                class="form-control"
                placeholder="Keterangan seperti: Pedas, Nasi Setengah, dan tambah acar"
                v-model="pesanan.keterangan"
              ></textarea>
            </div>
            <button type="submit" class="btn btn-success" @click="pemesanan">
              <b-icon-cart></b-icon-cart> Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar.vue";
import axios from "axios";
export default {
  name: "FoodDetail",
  components: {
    Navbar,
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
    pemesanan() {
      if (this.pesanan.jumlah_pemesanan) {
        this.pesanan.products = this.product;
        axios
          .post("http://localhost:3000/keranjangs", this.pesanan)
          .then(() => {
            this.$toast.success("Pesanan dimasukkan ke keranjang", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
            this.resetForm();
            this.$router.push({ path: "/keranjang" });
          })
          .catch((err) => console.log(err));
      } else {
        this.$toast.error("Jumlah pesanan masih kosong", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
    resetForm() {
      (this.pesanan.jumlah_pemesanan = ""), (this.pesanan.keterangan = "");
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then((response) => {
        // handle success
        this.setProduct(response.data);
      })
      .catch((error) => {
        // handle error
        console.log("Failed: ", error);
      })
      .then(function () {
        // always executed
      });
  },
};
</script>

<style></style>
