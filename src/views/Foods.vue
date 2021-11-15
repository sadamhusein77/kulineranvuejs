<template>
  <div class="foods">
    <Navbar />
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <h2>Daftar <strong>Makanan</strong></h2>
        </div>
      </div>
      <div class="row mt-2">
        <div class="col">
          <div class="input-group mb-3">
            <input
              v-model="search"
              type="text"
              class="form-control"
              placeholder="Search"
              aria-label="Search"
              aria-describedby="basic-addon1"
              @keyup="searchFood"
            />
            <div class="input-group-prepend">
              <span class="input-group-text" id="basic-addon1"
                ><b-icon-search></b-icon-search
              ></span>
            </div>
          </div>
        </div>
      </div>
      <div class="row mb-4">
        <div
          class="col-md-4 mt-4"
          v-for="product in products"
          :key="product.id"
        >
          <CardProduct :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar.vue";
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";

export default {
  name: "Foods",
  components: { Navbar, CardProduct },
  data() {
    return {
      products: [],
      search: "",
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
    searchFood() {
      axios
        .get("http://localhost:3000/products?q=" + this.search)
        .then((response) => {
          // handle success
          this.setProducts(response.data);
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
      .get("http://localhost:3000/products")
      .then((response) => {
        // handle success
        this.setProducts(response.data);
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
