<template>
    <Form @submit="submitUser" :validation-schema="contactFormSchema">
        <h2 v-if="!contactLocal._id">Đăng ký</h2>
        <h2 v-if="contactLocal._id">Chỉnh sửa thông tin</h2>
        <div class="input-section">
            <Field name="name" type="text" class="form-control" v-model="contactLocal.name" placeholder="Nhập họ tên" />
            <ErrorMessage name="name" class="error-feedback" />
        </div>
        <div class="input-section">
            <Field name="email" type="email" class="form-control" v-model="contactLocal.email" placeholder="Nhập email" />
            <ErrorMessage name="email" class="error-feedback" />
        </div>
        <div class="input-section">
            <Field name="address" type="text" class="form-control" v-model="contactLocal.address"
                placeholder="Nhập địa chỉ" />
            <ErrorMessage name="address" class="error-feedback" />
        </div>

        <div class="input-section">
            <Field name="phone" type="phone" class="form-control" v-model="contactLocal.phone"
                placeholder="Nhập số điện thoại" />
            <ErrorMessage name="phone" class="error-feedback" />
        </div>

        <div class="input-section">
            <Field name="password" type="password" class="form-control" v-model="contactLocal.password"
                placeholder="Nhập mật khẩu" />
            <ErrorMessage name="password" class="error-feedback" />
        </div>
        <button class="btn btn-primary" id="btn-register" v-if="!contactLocal._id">Đăng ký</button>
        <button class="btn btn-primary" v-if="contactLocal._id">Lưu</button>
    </Form>
</template>

<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";
import ContactService from "@/services/bookstore.service";

export default {
    components: {
        Form,
        Field,
        ErrorMessage,
    },
    emits: ["submit:contact",],
    props: {
        contact: { type: Object, required: true },

    },
    data() {
        const contactFormSchema = yup.object().shape({
            name: yup
                .string()
                .required("Tên phải có giá trị.")
                .min(2, "Tên phải ít nhất 2 ký tự")
                .max(50, "Tên nhiều nhất có 50 ký tự."),
            email: yup
                .string()
                .email("Email không đúng")
                .max(50, "Email tối đa 50 ký tự"),
                
                
            address: yup.string().max(100, "Địa chỉ tối đa 100 ký tự"),
            phone: yup
                .string()
                .matches(
                    /((09|03|07|08|05)+([0-9]{8})\b)/g,
                    "Số điện thoại không hợp lệ"
                )

        });
        return {
            contactLocal: this.contact,
            contactFormSchema,
            listContact: [],
        };
    },
    methods: {
        submitUser() {
            for(let i = 0; i < this.listContact.length ; i++) {
                if(this.contactLocal._id == this.listContact[i]._id) break;
                    if(this.contactLocal.phone == this.listContact[i].phone) {
                        alert("Số điện thoại này đã tồn tại");
                        return;
                    } else if(this.contactLocal.email == this.listContact[i].email) {
                        alert("Email này đã tồn tại");
                        return;
                    }
                }
            this.$emit("submit:contact", this.contactLocal);
            
        },

        async retrieveContacts() {
            try {

                this.listContact = await ContactService.getAll();

            }
            catch (error) {
                console.log(error);
            }
        },
        filteredContacts() {
            for (let i = 0; i < this.listContact.length; i++) {
                if (this.contactLocal.phone == this.listContact[i].phone) {
                    return;
                }
            }
        },
    },
    computed: {
        contactStrings() {
            return this.listContact.map((listContact) => {
                const { name, email, address, phone } = listContact;
                return [name, email, address, phone].join("");
            });
        },

    },
    mounted() {
        this.retrieveContacts();
    }
};
</script>

<style scoped>
.form-control {
    margin: 10px 10px;
}
</style>