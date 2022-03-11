<template>
    <tr>
        <td>
            <a :href="productPath(data)">
                <img style="width: 50px; height: 50px" :src="data.Pic" alt="" />
            </a>
        </td>
        <td style="width: 55%">
            {{
                data.pSlogan == 1 ? data.PromotionSlogan : data.ProductNamePromo
            }}
            {{ data.ProductName }}
        </td>
        <td>
            <ul>
                <li>
                    <a
                        href="#"
                        class="btn_delete"
                        :id="data.ProductID"
                        @click.prevent="remove"
                        >刪除按鈕
                        <span class="glyphicon glyphicon-remove-circle"></span>
                    </a>
                </li>
                <li style="margin-top: -10px; margin-bottom: -5px">
                    x {{ data.Qty }}
                </li>
                <li v-if="data.BonusProduct == 0">NT$ {{ subtotal }}</li>
                <li v-else>{{ subtotal }} 點</li>
            </ul>
        </td>
    </tr>
</template>

<script>
export default {
    props: {
        data: {
            require: true,
            type: Object,
        },
    },
    computed: {
        productPath() {
            return (item) => {
                let path =
                    item.BonusProduct == '0'
                        ? 'product.php?pid='
                        : 'bonus_third.php?pid=';

                return path + this.data.ProductID;
            };
        },
        subtotal() {
            // 確認這兩個計算欄位???
            return parseInt(this.data.BargainPrice) * parseInt(this.data.Qty);
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
