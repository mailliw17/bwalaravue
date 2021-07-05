<template> 
     <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
        <div class="container">
            <div class="row">
                <div class="col-lg-8">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="cart-table">
                                <table>
                                    <thead>
                                        <tr>
                                            <th>Image</th>
                                            <th class="p-name text-center">Product Name</th>
                                            <th>Price</th>
                                            <th>Action</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="keranjang in keranjangUser" :key="keranjang.id">
                                            <td class="cart-pic first-row">
                                                <img class="img-cart" v-bind:src="keranjang.photo" />
                                            </td>
                                            <td class="cart-title first-row text-center">
                                                <h5>{{keranjang.name}}</h5>
                                            </td>
                                            <td class="p-price first-row">${{keranjang.price}}</td>
                                            <td class="delete-item" @click="removeItem(keranjang.id)">
                                                <a href="#">
                                                    <i class="material-icons">
                                                        close
                                                    </i>
                                                </a>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        <div class="col-lg-8 text-left">
                            <h4 class="mb-4">
                                Informasi Pembeli:
                            </h4>
                            <div class="user-checkout">
                                <form>
                                    <div class="form-group">
                                        <label for="namaLengkap">Nama lengkap</label>
                                        <input type="text" class="form-control" id="namaLengkap" aria-describedby="namaHelp" placeholder="Masukan Nama" v-model="customerInfo.name">
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">Email Address</label>
                                        <input type="email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email" v-model="customerInfo.email">
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">No. HP</label>
                                        <input type="number" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP" v-model="customerInfo.number">
                                    </div>
                                    <div class="form-group">
                                        <label for="alamatLengkap">Alamat Lengkap</label>
                                        <textarea class="form-control" id="alamatLengkap" rows="3" v-model="customerInfo.address"></textarea>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4 text-left">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="proceed-checkout">
                                <ul>
                                    <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                                    <li class="subtotal mt-3">Subtotal <span>${{totalHarga}}.00</span></li>
                                    <li class="subtotal mt-3">Pajak <span>10%</span></li>
                                    <li class="subtotal mt-3">Total Biaya <span>${{totalBiaya}}.00</span></li>
                                    <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                                    <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                                    <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                                </ul>
                                <!-- <router-link to="/success" class="proceed-btn">  -->
                                    <a @click="checkout()" href="#" class="proceed-btn">I ALREADY PAID</a>
                                <!-- </router-link> -->
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Shopping Cart Section End -->
</template>

<script>
// import Header from "@/components/Header.vue";
import axios from "axios";

export default {
    name:'ShoppingCart',
    data() {
        return {
            keranjangUser: [],
            customerInfo : {
                name : '',
                email: '',
                number : '',
                address: ''
            }
        };
    },
    methods: {
        changeImage(urlImage) {
            this.gambar_default = urlImage;
        },
        setDataPicture(data) {
            // replace objek productDetails dengan data dari API
            this.productDetails = data
            
            // replace value gambar default dengan data dari API (galleries)
            this.gambar_default = data.galleries[0].photo;
        },
        saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct) {

            var productStored = {
                "id" : idProduct,
                "name": nameProduct,
                "price":priceProduct, 
                "photo" : photoProduct
            }
            this.keranjangUser.push(productStored);
            const parsed = JSON.stringify(this.keranjangUser);
            localStorage.setItem('keranjangUser', parsed);
        },
         removeItem(idx) {
            // cari tahu id dari item mana yang akan dihapus
            let keranjangUserStorage = JSON.parse(localStorage.getItem("keranjangUser"));
            let itemKeranjangUserStorage = keranjangUserStorage.map(itemKeranjangUserStorage => itemKeranjangUserStorage.id);

            // cocokan idx item dengan id yang ada di storage
            let index = itemKeranjangUserStorage.findIndex(id => id == idx);
            this.keranjangUser.splice(index, 1);

            // update ke local storage
            const parsed = JSON.stringify(this.keranjangUser);
            localStorage.setItem("keranjangUser", parsed);
            window.location.reload();
        },
        checkout() {
            let productIds = this.keranjangUser.map(function(product){
                return product.id
            });

            let checkoutData = {
                'name' : this.customerInfo.name,
                'email': this.customerInfo.email,
                'number' : this.customerInfo.number,
                'address': this.customerInfo.address,
                'transaction_total' : this.totalBiaya,
                'transaction_status' : "PENDING",
                'transaction_details' : productIds
            };

            axios
                .post("http://localhost:8000/api/checkout", checkoutData)
                // jika berhasil lempar ke halaman success
                .then(() => this.$router.push('success'))
                // eslint-disable-next-line no-console
                .catch(err =>console.log(err));
        }
    },
    mounted() {
            if (localStorage.getItem('keranjangUser')) {
                try {
                    this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
                } catch(e) {
                    localStorage.removeItem('keranjangUser');
                }
            }
        },
    computed: {
        totalHarga() {
            return this.keranjangUser.reduce(function(items, data){
                return items + data.price;
            },0);
        },
        ditambahPajak() {
            return (this.totalHarga * 10) / 100;
        },
        totalBiaya() {
            return this.totalHarga + this.ditambahPajak;
        }
    }
}
</script>

<style scoped>
    .img-cart {
        height: 100px;
        width: 100px;
    }
</style>