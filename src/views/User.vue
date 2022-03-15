<template>
  <main role="main">
    <div class="album py-5 bg-light">
      <div class="container">
        <div class="card mb-3">
          <!---userprofile--->
          <UserProfileCard
            :initialProfile="profile"
            :Comments="Comments"
            :FavoritedRestaurants="FavoritedRestaurants"
            :Followers="Followers"
            :Followings="Followings"
          />
        </div>

        <div class="row">
          <div class="col-md-4">
            <!---following--->
            <UserFollowingsCard :Followings="Followings" />
            <br />
            <!--follows-->
            <UserFollowersCard :Followers="Followers" />
          </div>
          <div class="col-md-8">
            <!--user comment cards-->
            <UserCommentsCard :Comments="Comments" />
            <br />
            <!--user favorite restaurant card-->
            <UserFavoritedRestaurantsCard
              :FavoritedRestaurants="FavoritedRestaurants"
            />
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
<script>
import UserProfileCard from "./../components/UserProfileCard";
import UserFollowingsCard from "./../components/UserFollowingsCard";
import UserFollowersCard from "./../components/UserFollowersCard";
import UserCommentsCard from "./../components/UserCommentsCard";
import UserFavoritedRestaurantsCard from "./../components/UserFavoritedRestaurantsCard";
import { Toast } from "./../utils/helpers";
import usersAPI from './../apis/users'

export default {
  components: {
    UserProfileCard,
    UserFollowingsCard,
    UserFollowersCard,
    UserCommentsCard,
    UserFavoritedRestaurantsCard,
  },
  data() {
    return {
      profile: {
        id: -1,
        name: "",
        image: "",
        email: "",
        isFollowed: true,
      },
      Comments: [],
      FavoritedRestaurants: [],
      Followers: [],
      Followings: [],
    };
  },
  created() {
    this.fetchUsers(this.$route.params.id);
  },
  beforeRouteUpdate(to,from,next){
    this.fetchUsers(to.params.id);
    next()
  },
  methods: {
    async fetchUsers(userId) {
      try {
        const { data } = await usersAPI.get({userId})
        this.profile = {
          id: data.profile.id,
          name: data.profile.name,
          image: data.profile.image,
          email: data.profile.email,
          isFollowed: data.isFollowed,
        };
        this.Comments = data.profile.Comments;
        this.FavoritedRestaurants = data.profile.FavoritedRestaurants;
        this.Followers = data.profile.Followers;
        this.Followings = data.profile.Followings;
      } catch (error) {
        Toast.fire({
          icon: "error",
          title: "無法取得使用者資料，請稍後再試",
        });
      }
    },
  },
};
</script>