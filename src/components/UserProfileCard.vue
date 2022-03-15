<template>
  <div class="row no-gutters">
    <div class="col-md-4">
      <img :src="profile.image" width="300px" height="300px" />
    </div>
    <div class="col-md-8">
      <div class="card-body">
        <h5 class="card-title">{{ profile.name }}</h5>
        <p class="card-text">{{ profile.email }}</p>
        <ul class="list-unstyled list-inline">
          <li>
            <strong>{{ Comments | length }}</strong> 已評論餐廳
          </li>
          <li>
            <strong>{{ FavoritedRestaurants | length }}</strong> 收藏的餐廳
          </li>
          <li>
            <strong>{{ Followers | length }}</strong> followings (追蹤者)
          </li>
          <li>
            <strong>{{ Followings | length }}</strong> followers (追隨者)
          </li>
        </ul>
        <p></p>
        <p class="mt-3">
          <router-link
            :to="{ name: 'user-edit', params: { id: currentUser.id } }"
            type="button"
            class="btn btn-primary"
            v-if="currentUser.id === profile.id"
          >
            編輯
          </router-link>
        </p>

        <p class="mt-3" v-if="currentUser.id !== profile.id">
          <button
            type="button"
            class="btn btn-danger"
            @click.stop.prevent="deleteFollowed(profile.id)"
            v-if="profile.isFollowed"
          >
            取消追蹤
          </button>
          <button
            type="button"
            class="btn btn-primary"
            @click.stop.prevent="addFollowed(profile.id)"
            v-else
          >
            追蹤
          </button>
        </p>
        <p></p>
      </div>
    </div>
  </div>
</template>
<script>
import { mapState } from "vuex";
import { Toast } from "./../utils/helpers";
import usersAPI from './../apis/users'
export default {
  props: {
    initialProfile: {
      type: Object,
      required: true,
    },
    Comments: {
      type: Array,
      required: true,
    },
    FavoritedRestaurants: {
      type: Array,
      required: true,
    },
    Followers: {
      type: Array,
      required: true,
    },
    Followings: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      profile: this.initialProfile,
    };
  },
  computed: {
    ...mapState(["currentUser"]),
  },
  watch:{
    initialProfile(newValue) {
      this.profile = {
        ...this.profile,
        ...newValue,
      };
    },
  },
  methods: {
   async addFollowed(userId) {
     try {
       const { data } = await usersAPI.addFollowing({ userId });
       if (data.status !== "success") {
          throw new Error(data.message);
        }
       this.profile = {
        ...this.profile,
        isFollowed: true,
      };
     } catch (error) {
       Toast.fire({
          icon: "error",
          title: "無法追隨，請稍後再試",
        });
     }
      
    },
   async deleteFollowed(userId) {
     try {
       const { data } = await usersAPI.deleteFollowing({ userId });
       if (data.status !== "success") {
          throw new Error(data.message);
        }
      this.profile = {
        ...this.profile,
        isFollowed: false,
      }; 
     } catch (error) {
       Toast.fire({
          icon: "error",
          title: "無法取消追隨，請稍後再試",
        });
     }
      
    },
  },
  filters: {
    length(target) {
      return target.length;
    },
  },
};
</script>