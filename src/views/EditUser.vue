<template>
  <div class="container py-5">
    <EditUserForm :initial-user="user" @after-submit="handleAfterSubmit" />
  </div>
</template>
<script>
import EditUserForm from "./../components/EditUserForm.vue";
import { mapState } from "vuex";
import usersAPI from "../apis/users"
import { Toast } from "../utils/helpers"
export default {
  components: {
    EditUserForm,
  },
  data() {
    return {
      user: {
        id: -1,
        name: "",
        image: "",
      },
    };
  },
  computed: {
    ...mapState(["currentUser"]),
  },
  created() {
    this.fetchUser();
  },
  watch: {
    currentUser() {
      this.fetchUser();
    },
  },
  methods: {
    fetchUser() {
      if (this.currentUser.id !== this.$route.params.id) {
        this.$router.push("not-found");
        return;
      }
      this.user = {
        ...this.user,
        id: this.currentUser.id,
        name: this.currentUser.name,
        image: this.currentUser.image,
      };
    },
    async handleAfterSubmit(formData) {
      // 透過 API 將表單資料送到伺服器
      try {
        const { data } = await usersAPI.update({
          userId: this.user.id,
          formData,
        });
        if (data.status === "error") {
          throw new Error(data.message);
        }
        this.$router.push({ name: "user", params: { id: this.user.id } });
      } catch (error) {
        this.isProcessing = false;
        Toast.fire({
          icon: "error",
          title: "無法更新資料，請稍後再試",
        });
      }
    },
  },
};
</script>