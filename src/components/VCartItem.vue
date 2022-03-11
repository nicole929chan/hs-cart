<template>
    <div>
        <h2>{{ data.Title }}</h2>
        <div>{{ data.BargainPrice }}</div>
        <div>{{ data.Qty }}</div>
        <button @click="remove">del</button>
    </div>
</template>

<script>
export default {
    props: {
        data: {
            require: true,
            type: Object,
        },
    },
    methods: {
        remove() {
            const subtotal =
                parseInt(this.data.BargainPrice) * parseInt(this.data.Qty) * -1;

            this.$emit('removed', {
                productID: this.data.ProductID,
                amount: subtotal,
            });
        },
    },
};
</script>
