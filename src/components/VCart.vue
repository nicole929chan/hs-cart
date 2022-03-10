<template>
    <div class="cart_content">
        <h1>{{ cart.title }}</h1>
        <v-cart-item></v-cart-item>
        <h2>合計: ${{ total }}</h2>
    </div>
</template>

<script>
import VCartItem from './VCartItem.vue';
export default {
    props: {
        data: {
            type: Object,
        },
    },
    components: {
        VCartItem,
    },
    data() {
        return {
            cart: this.data,
            total: 0,
        };
    },
    mounted() {
        // cart.items.reduce()
    },
};
</script>
