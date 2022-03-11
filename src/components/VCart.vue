<template>
    <div class="cart_content">
        <div :style="flexBetween">
            <div :style="flexDefault">
                <h1 @click="display = !display">{{ title }}</h1>
                <h2>$xxx免運</h2>
                <h2>({{ items.length }})</h2>
            </div>
            <h2>NT${{ subtotal }}</h2>
        </div>
        <table v-show="display">
            <v-cart-item
                v-for="item in items"
                :key="item.ProductID"
                :data="item"
                @removed="removeItem"
            ></v-cart-item>
        </table>
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
            flexDefault: {
                display: 'flex',
            },
            flexBetween: {
                display: 'flex',
                'justify-content': 'space-between',
            },
            display: false,
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
