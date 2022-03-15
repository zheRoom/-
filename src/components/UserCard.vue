<template>
  <div class="col-3">
    <router-link :to="{ name: 'user', params: { id: user.id } }">
      <img :src="user.image" width="140px" height="140px" />
    </router-link>
    <h2>{{ user.name }}</h2>
    <span class="badge badge-secondary"
      >追蹤人數：{{ user.followerCount }}</span
    >
    <p class="mt-3">
      <button
        type="button"
        class="btn btn-danger"
        @click.stop.prevent="deleteFollowed(user.id)"
        v-if="user.isFollowed"
      >
        取消追蹤
      </button>
      <button
        type="button"
        class="btn btn-primary"
        @click.stop.prevent="addFollowed(user.id)"
        v-else
      >
        追蹤
      </button>
    </p>
  </div>
</template>
<script>
import usersAPI from "./../apis/users";
import { Toast } from "./../utils/helpers";
export default {
  props: {
    userdata: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      user: this.userdata,
    };
  },
  methods: {
    async addFollowed(userId) {
      try {
        const { data } = await usersAPI.addFollowing({ userId });

        if (data.status !== "success") {
          throw new Error(data.message);
        }
        this.user = {
          ...this.user, // 保留餐廳原有資料
          followerCount: this.user.followerCount + 1,
          isFollowed: true,
        };
      } catch(error) {
        Toast.fire({
          icon: "error",
          title: "無法加入追蹤，請稍後再試",
        });
      }
    },
    async deleteFollowed(userId) {
      try {
        const { data } = await usersAPI.deleteFollowing({ userId });

        if (data.status !== "success") {
          throw new Error(data.message);
        }
        this.user = {
          ...this.user, // 保留餐廳原有資料
          followerCount: this.user.followerCount - 1,
          isFollowed: false,
        };
      } catch(error) {
        Toast.fire({
          icon: "error",
          title: "無法刪除追蹤，請稍後再試",
        });
      }
    },
  },
};
</script>