<template>
    <div>總計: ${{ total }}</div>
</template>

<script>
export default {
    props: {
        total: {
            require: true,
            type: Number,
        },
    },
};
</script>
