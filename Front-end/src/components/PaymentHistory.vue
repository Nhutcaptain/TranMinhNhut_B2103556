<template>
    <ul class="history-list">
        <li class="history-item" v-for="(item, index) in sortedOrderBill" :key="item._id">
            <div class="details">
                <div class="history-item-img">
                    <img :src="item.img" alt="">
                </div>
                <div class="items name">
                    <strong>Tên sách</strong>
                    {{ item.name }}
                </div>
                <div class="items counts">
                    <strong>Số lượng</strong>
                    {{ item.count }}
                </div>
                <div class="items price">
                    <strong>Tổng giá</strong>
                    {{ item.price * item.count }}
                </div>
                <div class="items date">
                    <strong>Ngày thanh toán</strong>
                    {{ item.ngaythem }}
                </div>
            </div>
        </li>
    </ul>
</template>
<script>
    export default {
        props: {
            orderBill: {type: Object, default: []},
        },
        computed: {
            sortedOrderBill() {
                return this.orderBill.slice().reverse();
            }
        },
        methods: {
            check() {
                console.log("hello");
                console.log(this.orderBill);
            },
        },
        mounted() {
            this.check();
        }
    }
</script>
<style scoped>
    .history-list{
        list-style: none;
    }
    .history-item-img{
        max-width: 120px;
        max-height: 150px;
        & img{
            max-width: 100%;
            max-height: 100%;
        }
    }
    .history-item{
        margin-top: 10px;
        position: relative;
        list-style: none;
        border: 2px solid rgb(142, 142, 249);
        border-radius: 5px;
    }
    .items{
        display: flex;
        flex-direction: column;
        text-align: center;
    }
    .details{
        display: flex;
        padding: 7px;
    }
    .name{
        width: 155px;
    }
</style>