<template>
  <div class="container py-5">
    <form @submit.stop.prevent="handleSubmit">
      <div class="form-group">
        <label for="name">Name</label>
        <input
          id="name"
          type="text"
          v-model="user.name"
          name="name"
          class="form-control"
          placeholder="Enter Name"
          required
        />
      </div>

      <div class="form-group">
        <label for="image">Image</label>
        <img
          v-if="user.image"
          :src="user.image"
          class="d-block img-thumbnail mb-3"
          width="200"
          height="200"
        />
        <input
          id="image"
          @change="handleFileChange"
          type="file"
          name="image"
          accept="image/*"
          class="form-control-file"
        />
      </div>

      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>
<script>
import { Toast } from "../utils/helpers"

export default {
  props: {
    initialUser: {
      type: Object,
      default: () => ({
        id: "",
        name: "",
        image: "",
      }),
    },
  },
  data() {
    return {
      user: {
        id: "",
        name: "",
        image: "",
      },
    };
  },
  created() {
    this.user = {
      ...this.user,
      ...this.initialUser,
    };
  },
  methods: {
    handleFileChange(e) {
      const { files } = e.target;

      if (files.length === 0) {
        // 使用者沒有選擇上傳的檔案
        this.user.image = "";
      } else {
        // 否則產生預覽圖
        const imageURL = window.URL.createObjectURL(files[0]);
        this.user.image = imageURL;
      }
    },
    handleSubmit(e) {
      if(!this.user.name) {
          Toast.fire({
            icon: 'warning',
            title: '尚未填寫使用者名稱'
          })
          return
        }
      const form = e.target; // <form></form>
      const formData = new FormData(form);
      this.$emit("after-submit", formData);
    },
  },
};
</script>