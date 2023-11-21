
<template>
    <!-- <div class="container">
        <div class="overlay-container">
            <div class="overlay">
                <div class="author regis" id="regis">
                    <h4>Đăng ký</h4>
                    <regisForm :contact="contact" @submit:contact="createContact" />
                    <p>{{ message }}</p>
                </div>
                <div class="author login" id="login">
                    <h4>Đăng nhập</h4>
                    <LoginForm :users="users" @submit:users="getUsers"/>
                </div>
            </div>
        </div>
    </div> -->
    <div class="welcome-app">
        <div class="container" id="container">
            <div class="form-container sign-up-container">
                <regisForm :contact="contact" @submit:contact="createContact" />
            </div>
            <div class="form-container sign-in-container">
                <LoginForm :users="users" @submit:users="getUsers" />
            </div>
            <div class="overlay-container">
                <div class="overlay">
                    <div class="overlay-panel overlay-left">
                        <h1>Xin chào!</h1>
                        <p>Nếu bạn đã đăng ký thành công. Mời bạn quay lại trang đăng nhập.</p>
                        <button class="ghost" id="signIn" @click="retriveUser">Đăng nhập</button>
                    </div>
                    <div class="overlay-panel overlay-right">
                        <h1>Xin chào!</h1>
                        <p>Bạn vui lòng đăng nhập trước khi sử dụng dịch vụ của chúng tôi</p>
                        <h4>Nếu bạn chưa có tài khoản, hãy đăng ký tài khoản mới</h4>
                        <button class="ghost" id="signUp" @click="toggleTab">Đăng ký</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import regisForm from "@/components/regisForm.vue";
import ContactService from "@/services/bookstore.service";
import LoginForm from "../components/loginForm.vue";
import toggleTab from "../script/toggleTab";
import Swal from 'sweetalert2'


export default {
    components: {
        regisForm,
        LoginForm,
    },
    data() {
        return {
            contact: {
                name: "",
                email: "",
                address: "",
                phone: "",
                password: "",
            },
            message: "",
            users: {
                email: "",
                password: "",
            },
            listUsers: [],
        };
    },

    methods: {
        createContact() {
            try {
                ContactService.create(this.contact);
                this.message = "Đã tạo tài khoản mới ";
                Swal.fire({
                        title: "Đăng ký thành công",
                        text: "You clicked the button!",
                        icon: "success",
                        allowOutsideClick: false,
                        showConfirmButton: true
                    })
            }
            catch (error) {
                console.log(error);
            }
        },
        getUsers() {
            let test = 0;
            for (let i = 0; i < this.listUsers.length; i++) {
                if (this.users.email == this.listUsers[i].email && this.users.password == this.listUsers[i].password) {
                    test = 1;
                    Swal.fire({
                        title: "Đăng nhập thành công",
                        text: "You clicked the button!",
                        icon: "success",
                        allowOutsideClick: false,
                        showConfirmButton: true
                    }).then((result) => {
                        if (result.isConfirmed) {
                            this.$router.push({
                                name: "home",
                                params: {
                                    id: this.listUsers[i]._id,
                                }
                            });
                        }
                    })

                } else {
                    continue;
                }
            }
            if (test == 0) {
                    Swal.fire({
                        icon: "warning",
                        title: "Oops...",
                        text: "Email hoặc mật khẩu sai rồi!",
                    });
                }
        },

        async retriveUser() {
            try {
                this.listUsers = await ContactService.getAll();
            }
            catch (error) {
                console.log(error);
            }
        },
    },
    mounted() {
        this.retriveUser();
        toggleTab();
    },
}
</script>