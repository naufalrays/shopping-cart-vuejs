<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <h1>Semua Produk</h1>
                <table class="table">
                    <thead>
                        <tr>
                            <th>Name</th>
                            <th>Description</th>
                            <th>Stock</th>
                            <th>Price</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(allProduct, index) in allProducts" :key="index" class="table align-middle">
                            <td>{{ allProduct.name }}</td>
                            <td>{{ allProduct.description }}</td>
                            <td>{{ allProduct.stock }}</td>
                            <td>Rp. {{ allProduct.price }}</td>
                            <td>
                                <button class="btn btn-primary" type="submit" @click="functionAddToCart(allProduct)"
                                    v-if="allProduct.stock > 0">
                                    Add to cart
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table>
                <ShoppingCartComponent :shoppingCartDatas="cartProducts" :totalPassing="total" @emit-click="functionDelete"
                    @emit-checkout="functionCheckout"></ShoppingCartComponent>
            </div>
        </div>
    </div>
</template>

<script>
import ShoppingCartComponent from './ShoppingCartComponent.vue';

export default {
    data: function () {
        return {
            allProducts: [
                {
                    name: "Indomie Goreng Rendang",
                    description: "Masakan instan terenak di dunia",
                    stock: 10,
                    price: 3900,
                },
                {
                    name: "Mie Gelas Rendang",
                    description: "Mie instant khusus anak kosan",
                    stock: 5,
                    price: 1500,
                },
                {
                    name: "Bakmi Mewah",
                    description: "Kalau anak kosan jangan macem2 deh",
                    stock: 80,
                    price: 10000,
                },
            ],
            cartProducts: [],
            total: 0,
            dataPass: "Data Passing 222",
            newStock: 0,
        };
    },
    methods: {
        functionAddToCart(allProducts) {
            allProducts.stock--;
            const existingIndex = this.cartProducts.findIndex(item => item.name === allProducts.name);
            if (existingIndex >= 0) {
                this.cartProducts[existingIndex].stock++;
                this.cartProducts[existingIndex].price += allProducts.price;
            }
            else {
                this.cartProducts.push({
                    name: allProducts.name,
                    description: allProducts.description,
                    stock: 1,
                    price: allProducts.price,
                });
            }
            this.total += allProducts.price;
        },
        functionDelete(index) {
            // console.log(this.cartProducts[index].price);
            this.total = this.total - this.cartProducts[index].price;
            this.allProducts.forEach((data) => {
                if (data.name === this.cartProducts[index].name) {
                    data.stock += this.cartProducts[index].stock
                };
            },);
            this.cartProducts.splice(index, 1);
            // this.total = this.total - this.cartProducts[index].price;
            console.log(index);
        },
        functionCheckout() {
            alert(`Pay us ${this.total}`)
        },
    },
    mounted() {
        console.log("Component mounted.");
    },
    components: { ShoppingCartComponent }
};
</script>
