<template>
  <div class="card">
    <div class="card-header">
      <strong>{{ Comments | length }}</strong> 已評論餐廳
    </div>
    <div class="card-body">
      <router-link
        :to="{ name: 'restaurant', params: { id: Comment.id } }"
        v-for="Comment in Comments"
        :key="Comment.id"
        class="ml-1"
      >
        <img :src="Comment.Restaurant.image" width="60" height="60" class="avatar" />
      </router-link>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    Comments: {
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