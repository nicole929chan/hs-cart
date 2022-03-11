<template>
    <div class="cart_content" style="border: 1px solid red">
        <h1>{{ title }}</h1>
        <v-cart-item
            v-for="item in items"
            :key="item.ProductID"
            :data="item"
            @removed="removeItem"
        ></v-cart-item>
        <h2>合計: ${{ subtotal }}</h2>
    </div>
</template>

<script>
import VCartItem from './VCartItem.vue';
export default {
    props: {
        data: {
            require: true,
            type: Object,
        },
    },
    components: {
        VCartItem,
    },
    data() {
        return {
            type: this.data.type,
            title: this.data.title,
            items: this.data.items,
            subtotal: 0,
        };
    },
    mounted() {
        this.calculate();
    },
    methods: {
        calculate() {
            const items = this.items.map(
                (item) => parseInt(item.BargainPrice) * parseInt(item.Qty)
            );

            this.subtotal = items.reduce((prev, curr) => (curr += prev));
        },
        removeItem(payload) {
            const newItems = this.items.filter(
                (item) => item.ProductID !== payload.productID
            );
            this.items = newItems;

            if (newItems.length > 0) {
                this.calculate();
            } else {
                this.$emit('remove', this.type);
            }

            this.$emit('increment', payload.amount);
        },
    },
};
</script>
