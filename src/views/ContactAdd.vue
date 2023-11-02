<template>
  <div class="page">
    <h4>Thêm mới Liên hệ</h4>
    <ContactFormAdd @submit:contact="createContact" />
    <p>{{ message }}</p>
  </div>
</template>
<script>
import ContactService from '@/services/contact.service';
import ContactFormAdd from '../components/ContactFormAdd.vue';
import Swal from 'sweetalert2';
export default {
  components: {
    ContactFormAdd
  },
  data() {
    return {
      message: ''
    };
  },
  methods: {
    async createContact(data) {
      try {
        await ContactService.create(data);
        Swal.fire('Good job!', 'Thêm liên hệ mới thành công!', 'success');
      } catch (error) {
        console.log(error);
      }
    }
  }
};
</script>
