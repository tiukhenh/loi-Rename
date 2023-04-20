<template>
    <div class="container col-8">
        <Form
            @submit="submitItem"
            :validation-schema="itemFormSchema"
        >
            <div class="form-group">
                <label for="ten">Tên</label>
                <Field
                    name="ten"
                    type="text"
                    class="form-control"
                    v-model="itemLocal.ten"
                />
                <ErrorMessage name="ten" class="error-feedback"/>
            </div>
            <div class="form-group">
                <label for="loai">Loại</label>
                <Field
                    name="loai"
                    type="text"
                    class="form-control"
                    v-model="itemLocal.loai"
                />
                <ErrorMessage name="loai" class="error-feedback"/>
            </div>        
            <div class="form-group">
                <label for="mauSac">Màu sắc</label>
                <Field
                    name="mauSac"
                    type="text"
                    class="form-control"
                    v-model="itemLocal.mauSac"
                />
                <ErrorMessage name="mauSac" class="error-feedback"/>
            </div>        
            <div class="form-group">
                <label for="gia">Giá</label>
                <Field
                    name="gia"
                    type="float"
                    class="form-control"
                    v-model="itemLocal.gia"
                />
                <ErrorMessage name="gia" class="error-feedback"/>
            </div>          
            <div class="form-group form-check">
                <input
                    name="tinhTrang"
                    type="checkbox"
                    class="form-check-input"
                    v-model="itemLocal.tinhTrang"
                />
                <label for="tinhTrang" class="form-check-label">
                    <strong>Tình trạng</strong>
                </label>
            </div>
            <div class="form-group">
                <button 
                    class="btn btn-violet">Lưu</button>
                <button
                    v-if="itemLocal._id"
                    type="button"
                    class="ml-2 btn btn-danger"
                    @click="deleteItem"
                >Xóa</button>
            </div>
        </Form>
</div>
</template>
<script>
import * as yup from "yup";
import { Form, Field, ErrorMessage} from "vee-validate";

export default {
    components: {
        Form,
        Field,
        ErrorMessage,
    },
    emits: ["submit:item", "delete:item"],
    props: {
        item: { type: Object, required:true}
    },
    data() {
        const itemFormSchema = yup.object().shape({
            ten: yup
                .string()
                .required("Tên phải có giá trị.")
                .min(2, "Tên phải ít nhất 2 ký tự.")
                .max(50, "Tên có nhiều nhất 50 ký tự."),
        });
        return {
            itemLocal: this.item,
            itemFormSchema,
        };
    },
    methods: {
        submitItem() {
            this.$emit("submit:item", this.itemLocal);
        },
        deleteItem() {
            this.$emit("delete:item", this.itemLocal.id);
        },

    },
};

</script>
<style>
</style>