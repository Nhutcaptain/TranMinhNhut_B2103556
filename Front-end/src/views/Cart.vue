
<template>
     <div class="title">
            <h2>GIỎ HÀNG</h2>
            <div class="history-groups" data-bs-toggle="modal"
                    data-bs-target="#myModalShowHistory">
                <i  class="history-titles">Xem lịch sử đặt hàng</i><i class="fa-solid fa-landmark"></i>
            </div>
        </div>
        
    <ShowCart :products="products" v-model:activeIndex="activeIndex" @refresh-orderbill="retriveOrderBill"></ShowCart>
    <div class="modal fade" id="myModalShowHistory">
            <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable modal-lg">
                <div class="modal-content">
                    <div class="modal-header">
                        <h2>Lịch sử thanh toán</h2>
                        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                    </div>
                    <div class="modal-body">
                        <div class="product-detail-body">
                            <PaymentHistory :orderBill="getPayment"></PaymentHistory>
                        </div>
                    </div>
                    <div class="modal-footer">
                    </div>
                </div>
            </div>
        </div>
    <div class="deleteAll" v-if="products.length != 0">
        <button class="btn btn-danger" @click="deleteAllCarts">Xóa tất cả sản phẩm trong giỏ hàng</button>
    </div>
</template>
<script>
import ShowCart from '../components/ShowCart.vue';
import bookstoreService from '../services/bookstore.service';
import PaymentHistory from '../components/PaymentHistory.vue';
import Swal from 'sweetalert2';


export default {
    components: {
        ShowCart,
        PaymentHistory,
    },
    props: {
        id: { type: String, required: true },
    },
    data() {
        return {
            products: [],
            activeIndex: -1,
            orderBill:[],
        }
    },
    computed: {
        getPayment() {
            return this.orderBill;
        }
    },
    methods: {
        async retriveCart() {
            try {
                this.products = await bookstoreService.getAllCart(this.id);
            } catch (error) {
                console.log(error);
            }
        },
        async deleteAllCarts() {
            Swal.fire({
                title: "Bạn chắc chắn chứ?",
                text: "Bạn có chắc là muốn xóa hết sản phẩm trong giỏ hàng không?",
                icon: "question",
                showConfirmButton: true,
            }).then((result) => {
                if(result.isConfirmed) {
                    console.log(this.id);
                    bookstoreService.deleteAllCart(this.id);
                    location.reload();
                }
            });
            
        },
        async retriveOrderBill() {
            this.orderBill = await bookstoreService.getPaymentHistory(this.id);
        }
    },

    mounted() {
        this.retriveCart();
        this.retriveOrderBill();
    },
}
</script>
<style scoped>
.title {
    position: relative;
    margin-top: 5px;
    padding: 10px 0;
    border: 1px solid;
    border-radius: 50px;
}
.history-groups{
    position: relative;
    left: 80%;
    border: 1px solid;
    width: 200px;
    top: -10px;
    padding: 10px;
    border-radius: 10px;
    cursor: pointer;
    &:hover{
        background-color: rgba(54, 55, 56, 0.144);
    }
}
.history-titles{
    margin-right: 5px;
}
</style>