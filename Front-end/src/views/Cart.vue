
<template>
    <ShowCart :products="products" v-model:activeIndex="activeIndex"></ShowCart>
    <div class="deleteAll" v-if="products.length != 0">
        <button class="btn btn-danger" @click="deleteAllCarts">Xóa tất cả sản phẩm trong giỏ hàng</button>
    </div>
</template>
<script>
import ShowCart from '../components/ShowCart.vue';
import bookstoreService from '../services/bookstore.service';
import Swal from 'sweetalert2';
export default {
    components: {
        ShowCart,
    },
    props: {
        id: { type: String, required: true },
    },
    data() {
        return {
            products: [],
            activeIndex: -1,
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
            
        }
    },

    mounted() {
        this.retriveCart();
    },
}
</script>