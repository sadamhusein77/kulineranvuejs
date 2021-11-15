<template>
  <div class="pesanan-success">
    <Navbar :updateKeranjang="keranjangs" />
    <div class="container">
      <div class="row justify-content-center mt-5">
        <div class="col text-center">
          <img src="assets/images/menunggu.png" alt="menunggu" width="300" />
          <h2><strong>Success!!</strong></h2>
          <h4>
            Pesanan Anda sedang diproses <br />
            Selamat Menunggu
          </h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Navbar from "../components/Navbar.vue";
export default {
  name: "PesananSuccess",
  components: { Navbar },
  data() {
    return {
      keranjangs: [],
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
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
};
</script>

<style></style>
