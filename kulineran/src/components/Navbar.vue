<template>
  <b-navbar toggleable="lg" type="dark" variant="light" sticky>
    <div class="container">
      <b-navbar-brand href="#" class="text-dark">AlthafRp</b-navbar-brand>

      <b-navbar-toggle
        target="nav-collapse"
        class="bg-success"
      ></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <li class="nav-item">
            <router-link class="nav-link text-dark" to="/">Home</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link text-dark" to="/foods"
              >Foods</router-link
            >
          </li>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <li class="nav-item">
            <router-link class="nav-link text-dark" to="/keranjang">
              keranjang
              <b-icon-bag></b-icon-bag>
              <span class="badge badge-success ml-2">{{
                updateKeranjang
                  ? updateKeranjang.length
                  : jumlah_pesanans.length
              }}</span>
            </router-link>
          </li>
        </b-navbar-nav>
      </b-collapse>
    </div>
  </b-navbar>
</template>

<script>
import axios from "axios";
export default {
  name,
  data() {
    return {
      jumlah_pesanans: [],
    };
  },
  props: ["updateKeranjang"],
  methods: {
    setJumlah(data) {
      this.jumlah_pesanans = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => this.setJumlah(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
</style>