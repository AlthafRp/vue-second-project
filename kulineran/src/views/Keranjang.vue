<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs"/>
    <div class="container">
        <div class="row">
            <div class="col mt-4">
                <nav aria-label="breadcrumb">
                <ol class="breadcrumb">
                    <li class="breadcrumb-item"><router-link to="/">Home</router-link></li>
                    <li class="breadcrumb-item"><router-link to="/foods">Food</router-link></li>
                    <li class="breadcrumb-item active" aria-current="page">Keranjang</li>
                </ol>
                </nav>
            </div>
        </div>
        <div class="row">
            <div class="col">
                <h2>Keranjang <strong>Saya</strong></h2>
                <div class="table-responsive mt-4">
                    <table class="table">
                        <thead>
                            <tr>
                                <th scope="col">No</th>
                                <th scope="col">Foto</th>
                                <th scope="col">Keterangan</th>
                                <th scope="col">Makanan</th>
                                <th scope="col">Jumlah</th>
                                <th scope="col">Harga</th>
                                <th scope="col">Total Harga</th>
                                <th scope="col">Hapus</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(keranjang, index) in keranjangs" :key="keranjang.id">
                                <th>{{ index + 1 }}</th>
                                <td>
                                    <img :src="'../assets/image/' + keranjang.products.gambar" class="img-fluid shadow" width="250" alt="">
                                </td>
                                <td>{{ keranjang.keterangan }}</td>
                                <td>{{ keranjang.products.nama }}</td>
                                <td>{{ keranjang.jumlah }}</td>
                                <td align="center">Rp. {{ keranjang.products.harga }}</td>
                                <td align="center"><strong>Rp. {{ keranjang.products.harga*keranjang.jumlah }}</strong></td>
                                <td align="center" class="text-danger"><b-icon-trash @click="hapusKeranjang(keranjang.id)"></b-icon-trash></td>
                            </tr>

                            <tr>
                                <td colspan="6" align="right">
                                    <strong>Total Harga :</strong>
                                </td>
                                <td align="center">
                                    <strong>Rp. {{ totalHarga }}</strong>
                                </td>
                                <td></td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
        <div class="row justify-content-end">
            <form action="" class="mt-3" v-on:submit.prevent>
                <div class="form-group">
                    <label for="nama">Nama :</label>
                    <input type="text" class="form-control" v-model="pesan.nama">
                </div>
                <div class="form-group">
                    <label for="noMeja">Nomor Meja :</label>
                    <input type="text" class="form-control" v-model="pesan.noMeja">
                </div>
                <button type="submit" class="btn btn-success float-right" @click="checkout"><b-icon-cart></b-icon-cart> Pesan</button>
            </form>
        </div>
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue';
import axios from 'axios';
export default {
    name,
    components: { Navbar },
    data() {
        return {
            keranjangs : [],
            pesan: {}
        }
    },
    methods: {
        setkeranjangs(data) {
            this.keranjangs = data
        },
        hapusKeranjang(id) {
            axios.delete("http://localhost:3000/keranjangs/"+id)
            .then(() => {
                this.$toast.error('pesanan berhasil di hapus', {
                    type: 'error',
                    position: 'top-right',
                    duration: 3000,
                    dismissible: true
                })

                // detect after hapus
                axios.get("http://localhost:3000/keranjangs")
                .then((response) => this.setkeranjangs(response.data))
                .catch((error) => console.log(error))
            })
            .catch((error) => console.log(error))
        },
        checkout() {
            if (this.pesan.nama && this.pesan.noMeja) {
                this.pesan.keranjangs = this.keranjangs
                axios
                .post("http://localhost:3000/pesanans", this.pesan)
                .then(() => {

                    this.keranjangs.map(function (item) {
                        return axios
                        .delete("http://localhost:3000/keranjangs/"+ item.id)
                        .catch((error) => console.log(error));
                    })
                     
                    this.$router.push({ path: "/foods" })
                    this.$toast.success('pesanan segera di proses mohon tunggu!!', {
                        type: 'success',
                        position: 'top-right',
                        duration: 3000,
                        dismissible: true
                    })
                })
                .catch((err) => console.log(err));
            } else {
                this.$toast.error('nama dan nomor meja harus diisi', {
                    type: 'error',
                    position: 'top-right',
                    duration: 3000,
                    dismissible: true
                })
            }
        }
    },
    mounted() {
        axios.get("http://localhost:3000/keranjangs")
        .then((response) => this.setkeranjangs(response.data))
        .catch((error) => console.log(error))
    },
    computed: {
        totalHarga() {
            return this.keranjangs.reduce(function(items, data) {
                return items+(data.products.harga*data.jumlah)
            }, 0)
        }
    }
}
</script>

<style>

</style>