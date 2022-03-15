// ./src/views/RestaurantsTop.vue
<template>
  <div class="container py-5">
    <NavTabs />
    <h1 class="mt-5">人氣餐廳</h1>
    <TopRestaurants
      :toprest="restaurant"
      v-for="restaurant in restaurants"
      :key="restaurant.id"
    />
  </div>
</template>
<script>
import NavTabs from "./../components/NavTabs";
import TopRestaurants from "./../components/TopRestaurants";
import restaurantsAPI from "./../apis/restaurants";
import { Toast } from "./../utils/helpers";

export default {
  components: {
    NavTabs,
    TopRestaurants,
  },
  data() {
    return {
      restaurants: [],
    };
  },
  created() {
    this.fetchFeeds();
  },
  methods: {
    async fetchFeeds() {
      try {
        const { data } = await restaurantsAPI.getTopRestaurant();
        this.restaurants = data.restaurants;
      } catch (error){
        Toast.fire({
          icon: "error",
          title: "無法取得top10餐廳，請稍後再試",
        });
      }
    },
  },
};
</script>