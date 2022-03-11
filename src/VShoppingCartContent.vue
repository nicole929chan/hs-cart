<template>
    <img alt="Vue logo" src="./assets/logo.png" />
    <v-cart-top :total="total"></v-cart-top>
    <v-cart
        v-for="cart in carts"
        :key="cart.type"
        :data="cart"
        @remove="removeCart"
        @increment="incrementTotal"
    >
    </v-cart>
</template>

<script>
import axios from 'axios';
import VCart from './components/VCart.vue';
import VCartTop from './components/VCartTop.vue';

export default {
    name: 'VShoppingCartContent',
    components: {
        VCart,
        VCartTop,
    },
    data() {
        return {
            items: [],
            carts: [],
            total: 0,
        };
    },
    mounted() {
        let endpoint = 'http://localhost:8000/items';

        axios
            .get(endpoint)
            .then((res) => {
                this.items = res.data;
                this.buildCarts();
            })
            .catch((err) => console.log(err));
    },
    methods: {
        buildCarts() {
            // carts[0] = {
            //     type: 1,
            //     title: 'xxx購物車',
            //     items: [
            //         {
            //             ProductID: 'a01',
            //             Title: 'product a',
            //             Qty: 2,
            //            BargainPrice: "199"
            //         },
            //         {
            //             ProductID: 'b01',
            //             Title: 'product b',
            //             Qty: 1,
            //             BargainPrice: "64"
            //         }
            //     ]
            // }
            let currentType = 0;
            let total = 0;
            this.items.forEach((item) => {
                let cartType = parseInt(item.CartType);
                if (cartType !== currentType) {
                    this.carts.push({
                        type: cartType,
                        title: item.CartTitle,
                        items: [item],
                    });
                } else {
                    let index = this.carts.length - 1;
                    let newItems = [...this.carts[index].items, item];

                    this.carts[index] = {
                        ...this.carts[index],
                        items: newItems,
                    };
                }

                currentType = cartType;

                total += parseInt(item.BargainPrice) * parseInt(item.Qty);
            });

            this.incrementTotal(total);
        },
        incrementTotal(amount) {
            this.total += amount;
        },
        removeCart(cartType) {
            const newCarts = this.carts.filter(
                (cart) => cart.type !== cartType
            );

            this.carts = newCarts;
        },
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
