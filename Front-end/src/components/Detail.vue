<script>
import bookstoreService from '../services/bookstore.service';
import AddtoCart from './AddtoCart.vue';
    export default{
        components: {
            AddtoCart,
        },
        props: {
            productDetail: {type: Object, required: true},
            id: {type: String, required: true},
        },
        data() {
            return {
                isAdmin:false,
                user:[],
            }
        },
        methods: {
            async check() {
                this.user = await bookstoreService.get(this.id);
                this.isAdmin = this.user.admin;
            }
        },
        mounted() {
            this.check();
        }
    }
</script>

<template>
    <div class="detail-products">
        <div class="productDetail-img">
            <img :src="this.productDetail.img" alt="">
        </div>
    <div class="productDetail-detail">
        <div class="p-1">
            <strong>Tên:</strong>
            {{ this.productDetail.name }}
        </div>
        <div class="p-1">
            <strong>Tác giả:</strong>
            {{ this.productDetail.author }}
        </div>
        <div class="p-1">
            <strong>Giá:</strong>
            {{ this.productDetail.price }}
        </div>
        <div class="p-1">
            <strong>Mô tả</strong>
            {{ this.productDetail.describe }}
        </div>
    </div>
    <router-link 
        :to="{
            name: 'editor',
            params: {productId: this.productDetail._id,
                    userId: this.id},
        }">
        <div class="editor" v-if="this.isAdmin" data-bs-toggle="modal" data-bs-target="#myModal3">
            <i class="fa-solid fa-pen-to-square"></i>
        </div>
    </router-link>
    </div>
    
</template>
<style>
    .detail-products{
        display: flex;
        flex-direction: row;
    }

    .productDetail-img{
        max-width: 200px;
    }

    .productDetail-img img{
        width: 100%;
    }

    .productDetail-detail{
        margin-left: 10px;
    }

    .editor{
        position: absolute;
        top: 90%;
        right: 10%;
    }
</style>