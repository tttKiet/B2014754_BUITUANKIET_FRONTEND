<template>
  <FormContact @submit="submitContact" :validation-schema="contactFormSchema">
    <div class="form-group">
      <label for="name">Tên</label>
      <Field name="name" type="text" class="form-control" />
      <ErrorMessage name="name" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="email">E-mail</label>
      <Field name="email" type="email" class="form-control" />
      <ErrorMessage name="email" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="address">Địa chỉ</label>
      <Field name="address" type="text" class="form-control" />
      <ErrorMessage name="address" class="error-feedback" />
    </div>
    <div class="form-group">
      <label for="phone">Điện thoại</label>
      <Field name="phone" type="tel" class="form-control" />
      <ErrorMessage name="phone" class="error-feedback" />
    </div>
    <div class="form-group form-check my-2">
      <input name="favorite" type="checkbox" class="form-check-input" />
      <label for="favorite" class="form-check-label">
        <i>Liên hệ yêu thích</i>
      </label>
    </div>
    <div class="form-group">
      <button class="btn btn-primary">
        <i class="fa-solid fa-floppy-disk"></i>

        <span class="ms-2">Lưu</span>
      </button>
    </div>
  </FormContact>
</template>
<script>
import * as yup from 'yup';
import { Form as FormContact, Field, ErrorMessage } from 'vee-validate';
export default {
  components: {
    FormContact,
    Field,
    ErrorMessage
  },
  emits: ['submit:contact'],
  data() {
    const contactFormSchema = yup.object().shape({
      name: yup
        .string()
        .required('Tên phải có giá trị.')
        .min(2, 'Tên phải ít nhất 2 ký tự.')
        .max(50, 'Tên có nhiều nhất 50 ký tự.'),
      email: yup.string().email('E-mail không đúng.').max(50, 'E-mail tối đa 50 ký tự.'),
      address: yup.string().max(100, 'Địa chỉ tối đa 100 ký tự.'),
      phone: yup.string().matches(/((09|03|07|08|05)+([0-9]{8})\b)/g, 'Số điện thoại không hợp lệ.')
    });
    return {
      contactFormSchema
    };
  },
  methods: {
    submitContact(data) {
      this.$emit('submit:contact', data);
    }
  }
};
</script>
<style scoped>
@import '@/assets/form.css';
</style>
