<template>
    <ul class="cart-list">
        <div class="invoice-header">
        </div>
        <li class="cart-list-item" v-for="(invoice, index) in sortedOrderBill" :key="invoice._id"
            :class="{ active: index === activeIndex }">
            <div class="details">
                <div class="item username">
                    <strong>Tên khách hàng</strong>
                    <div class="click-username" @click="updateUserID(invoice.userID)">
                        {{ invoice.username }}
                    </div>
                </div>
                <div class="item name">
                    <strong>Tên sản phẩm</strong>
                    <div class="click-product-name" @click="updateProductID(invoice.productId)" data-bs-toggle="modal"
                    data-bs-target="#detailModal">
                        {{ invoice.name }}
                    </div>
                </div>
                <div class="item item-count">
                    <strong>Số lượng</strong>
                    {{ invoice.count }}
                </div>
                <div class="item item-price">
                    <strong>Giá</strong>
                    {{ invoice.price * invoice.count }}
                </div>
                <div class="item date">
                    <strong>Ngày thanh toán</strong>
                    {{ invoice.ngaythem }}
                </div>
                <div class="buttons">
                    <button class='btn btn-danger' @click="confirmed(index, invoice.userID, invoice.productId)" v-if="!invoice.confirmed">Xác nhận</button>
                    <button class="btn btn-secondary" v-if="invoice.confirmed">Đã xác nhận</button>
                </div>
                <div class="form-check2" v-if="getCheck && invoice.confirmed">
                <input name="deleteInvoice" type="checkbox" class="form-check-input2" :value="invoice._id"
                    v-model="selectedInvoices">
            </div>
            </div>
        </li>
    </ul>
</template>
<script>
import bookstoreService from '../services/bookstore.service';
export default {
    props: {
        orderBill: { type: Object, default: [] },
        activeIndex: {type: Number, default: -1},
        userID: { type: String, default: null },
        productId: {type: String, default: null},
        getCheck: { type: Boolean, default: false },
    },
    data() {
        return {
            cart:'',
            selectedInvoices:[],

        }
    },
    emits: ['update:userID', 'update:productId'],
    watch: {
        selectedInvoices(newValue){
            this.$emit('invoice-id-selected', newValue);
        },
    },
    computed: {
        sortedOrderBill() {
            // Sắp xếp orderBill theo index giảm dần
            return this.orderBill.slice().reverse();
        },
    },
    methods: {
        updateUserID(id) {
            this.$emit('update:userID', id);
        },
        async confirmed(index, userID, productId) {
            const temp = this.orderBill.slice().reverse();
            if(!temp[index].confirmed){
                this.cart = await bookstoreService.getOneCart(userID, productId);
                this.cart.vanchuyen = true;
                await bookstoreService.updateCart(userID, productId, this.cart);
                temp[index].confirmed = true;
                await bookstoreService.updateOrder(temp[index]._id, temp[index]);
            }
        },
        updateProductID(id) {
            this.$emit('update:productId', id);
        },
        deleteSelectedProducts() {
            // Gửi danh sách selectedProducts về ParentComponent
            this.$emit('delete-selected-products', this.selectedInvoices);
            // Đặt lại danh sách selectedProducts
            this.selectedInvoices = [];
        },
    },
}
</script>

<style scoped>
.cart-list {
    position: relative;
}

.cart-list-item {
    margin-top: 15px;
    position: relative;
    list-style: none;
    border: 2px solid rgb(142, 142, 249);
    border-radius: 5px;
    width: 750px;
}

.details {
    position: relative;
    display: flex;
    margin-left: 20px;
    max-width: 700px;
    width: 750px;
}

.item {
    margin-right: 10px;
    display: flex;
    flex-direction: column;
    width: 100px;
    border-right: 2px solid rgb(142, 142, 249);
    box-sizing: border-box;
    height: 80px;
}

.username {
    width: 130px;
    cursor: pointer;
}

.name {
    width: 120px;
    cursor: pointer;
}

.click-product-name:hover{
    background-color: aliceblue;
}

.date {
    width: 140px;
    align-items: center;
    text-align: center;
}

.item-price {
    width: 75px;
}

.item-count {
    text-align: center;
    width: 75px;
}

.buttons {
    position: relative;
    top: 5px;
}

.btn-danger {
    font-size: 13px;
    width: 110px;
}

.click-username:hover {
    width: 100%;
    background-color: aliceblue;
}

.btn-secondary{
    pointer-events: none;
    opacity: .7;
    width: 110px;
    font-size: 13px;
}
.form-check2{
    position: absolute;
    right: -4%;
    width: 20px;
    height: 15px;
    & input{
        width: 100%;
        height: 100%;
    }
}
</style>