<template>
     <Form
        @submit="submitUser"
        :validation-schema="contactFormSchema"
    >
    <h2>Đăng nhập</h2>
    <div class="input-section">
        <Field
            name="email"
            type= "email"
            class="user-input"
            v-model="userLocal.email"
            placeholder="Nhập emai"
        />
    </div>
    <ErrorMessage name="email" class="error-feedback"/>
    <div class="input-section">
        <Field
            name="password"
            type="password"
            class="user-input"
            v-model="userLocal.password"
            placeholder="Nhập mật khẩu"
        >
        </Field>
        
    </div>
    <ErrorMessage name="password" class="error-feedback"/>
    <p id="forgot-password">Quên mật khẩu</p>
    <div class="form-group">
        <button class="btn btn-primary" >Đăng nhập</button>
    </div>
    </Form>
    
</template>

<script>
    import {Form, Field, ErrorMessage} from "vee-validate";
    import bookstoreService from "../services/bookstore.service";
    import * as yup from "yup";

    export default {
        components: {
            Form,
            Field,
            ErrorMessage,
        },
        emits :["submit:users"],
        props: {
            users: {type: Object, require: true},
        },
        data() {
            const contactFormSchema = yup.object().shape({
                email: yup
                    .string()
                    .required("*Vui lòng điền email")
                    .email("Email không đúng")
                    .max(50, "Email tối đa 50 ký tự"),
                password: yup
                    .string()
                    .required("*Vui lòng điền password"),
                    
            });
            return {
                userLocal: this.users,
                contactFormSchema,
            };
        },
        methods: {
            submitUser() {
                // for(let i = 0; i < this.listUser.length; i++) {
                //     if(this.userLocal.email == this.listUser[i].email && this.userLocal.password == this.listUser[i].password){
                //         alert("Đăng nhập thành công");
                //         this.$emit("submit:user",this.userLocal);
                //     } else return;
                // }
                this.$emit("submit:users", this.userLocal);
                
            },       
        },
       
    }
</script>

<style scoped>
    .user-input{
        margin:10px 10px;
    }
    .error-feedback{
        color: red;
    }
</style>