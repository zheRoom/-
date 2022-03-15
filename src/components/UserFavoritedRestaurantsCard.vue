<template>
  <div class="card">
    <div class="card-header"><strong>{{FavoritedRestaurants | length}}</strong> 收藏的餐廳</div>
    <div class="card-body">
      <router-link
        :to="{ name: 'restaurant', params: { id: FavoritedRestaurant.id } }"
        v-for="FavoritedRestaurant in FavoritedRestaurants"
        :key="FavoritedRestaurant.id"
        class="ml-1"
      >
        <img
          :src="FavoritedRestaurant.image"
          width="60"
          height="60"
          class="avatar"
        />
      </router-link>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    FavoritedRestaurants: {
      type: Array,
      required: true,
    },
  },
  filters: {
    length(target) {
      return target.length;
    },
  },
};
</script>