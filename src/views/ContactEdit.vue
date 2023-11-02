<template>
  <div v-if="contact" class="page">
    <h4>Hiệu chỉnh Liên hệ</h4>
    <ContactForm
      :contact="contact"
      @submit:contact="updateContact"
      @delete:contact="deleteContact"
    />
    <p>{{ message }}</p>
  </div>
</template>
<script>
import ContactForm from '@/components/ContactForm.vue';
import ContactService from '@/services/contact.service';
import Swal from 'sweetalert2';
export default {
  components: {
    ContactForm
  },
  props: {
    id: { type: String, required: true }
  },
  data() {
    return {
      contact: null,
      message: ''
    };
  },
  methods: {
    async getContact(id) {
      try {
        this.contact = await ContactService.get(id);
      } catch (error) {
        console.log(error);
        this.$router.push({
          name: 'notfound',
          params: {
            pathMatch: this.$route.path.split('/').slice(1)
          },
          query: this.$route.query,
          hash: this.$route.hash
        });
      }
    },
    async updateContact(data) {
      try {
        await ContactService.update(this.contact._id, data);
        Swal.fire('Good job!', 'Liên hệ được cập nhật thành công', 'success');
      } catch (error) {
        console.log(error);
      }
    },
    async deleteContact() {
      Swal.fire({
        title: 'Bạn có muốn xóa liên hệ này?',
        showDenyButton: true,
        showCancelButton: false,
        confirmButtonText: 'Đồng ý',
        denyButtonText: `Hủy`
      }).then(async (result) => {
        /* Read more about isConfirmed, isDenied below */
        if (result.isConfirmed) {
          try {
            await ContactService.delete(this.contact._id);
            this.$router.push({ name: 'contactbook' });
            this.refreshList();
          } catch (error) {
            console.log(error);
          }
          Swal.fire('Đã xóa!', '', 'success');
        } else if (result.isDenied) {
          Swal.fire('Dữ liệu chưa được thay đổi', '', 'info');
        }
      });
    }
  },
  created() {
    this.getContact(this.id);
    this.message = '';
  }
};
</script>
