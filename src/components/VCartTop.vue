<template>
    <div :style="topStyle">
        <div>購物車明細</div>
        <div>累計 ${{ total }}</div>
    </div>
</template>

<script>
export default {
    props: {
        total: {
            require: true,
            type: Number,
        },
    },
    data() {
        return {
            topStyle: {
                display: 'flex',
                'justify-content': 'space-between',
                'font-size': '30px',
            },
        };
    },
};
</script>
