<template>
  <div class="card">
    <div class="card-header">最新餐廳</div>
    <div class="card-body">
      <div v-for="restdata in restaurants" :key="restdata.id">
        <h4>
          <router-link
            :to="{ name: 'restaurant', params: { id: restdata.id } }"
            >{{ restdata.name }}</router-link
          >
          <small>{{
            restdata.Category ? restdata.Category.name : "未分類"
          }}</small>
        </h4>
        <p>
          {{ restdata.description }}
          {{ restdata.createdAt | fromNow }}
        </p>

        <hr />
      </div>
    </div>
  </div>
</template>

<script>
import { fromNowFilter } from "./../utils/mixins";
export default {
  mixins: [fromNowFilter],
  props: {
    restaurants: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
small {
  font-size: 10px;
}
</style>