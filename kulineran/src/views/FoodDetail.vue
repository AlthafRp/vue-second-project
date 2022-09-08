<template>
  <div class="detailfood">
    <Navbar/>
    <div class="container">
        <div class="row">
            <div class="col mt-4">
                <nav aria-label="breadcrumb">
                <ol class="breadcrumb">
                    <li class="breadcrumb-item"><router-link to="/">Home</router-link></li>
                    <li class="breadcrumb-item"><router-link to="/foods">Food</router-link></li>
                    <li class="breadcrumb-item active" aria-current="page">Food Order</li>
                </ol>
                </nav>
            </div>
        </div>
        <div class="row">
            <div class="col-md-6">
                <img :src="'../assets/image/' + product.gambar" class="img-fluid" alt="">
            </div>
            <div class="col-md-6">
                <h2><strong>{{ product.nama }}</strong></h2>
                <hr>
                <h4>Harga : <strong>Rp. {{ product.harga }}</strong></h4>
                <form action="" class="mt-3" v-on:submit.prevent>
                    <div class="form-group">
                        <label for="jumlah">Jumlah Pesanan</label>
                        <input type="number" class="form-control" v-model="pesan.jumlah">
                    </div>
                    <div class="form-group">
                        <label for="keterangan">Keterangan Pesanan</label>
                        <textarea class="form-control" placeholder="silahkan kasih keterangan" v-model="pesan.keterangan"></textarea>
                    </div>
                    <button type="submit" class="btn btn-success" @click="pemesanan"><b-icon-cart></b-icon-cart> Pesan</button>
                </form>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue';
import axios from 'axios';
export default {
    components: { Navbar },
    data() {
        return {
        product: {},
        pesan: {}
        };
    },
    methods: {
        setProduct(data) {
        this.product = data;
        },
        pemesanan() {
            if (this.pesan.jumlah) {
                this.pesan.products = this.product;
                axios
                .post("http://localhost:3000/keranjangs", this.pesan)
                .then(() => {
                    this.$router.push({ path: "/keranjang" })
                    this.$toast.success('Berhasil masuk keranjang', {
                        type: 'success',
                        position: 'top-right',
                        duration: 3000,
                        dismissible: true
                    })
                })
                .catch((err) => console.log(err))
            } else {
                this.$toast.error('Jumlah Pesanan Harus di Isi', {
                    type: 'error',
                    position: 'top-right',
                    duration: 3000,
                    dismissible: true
                })
            }
        }
    },
    mounted() {
        axios.get("http://localhost:3000/products/"+this.$route.params.id)
        .then((response) => this.setProduct(response.data))
        .catch((error) => console.log(error))
    }
}
</script>

<style>

</style>