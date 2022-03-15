<template>
  <div class="card">
    <div class="card-header">
      <strong>{{ Followers | length }}</strong> followers (追隨者)
    </div>
    <div class="card-body">
      <router-link
        :to="{ name: 'user', params: { id: Follower.id } }"
        v-for="Follower in Followers"
        :key="Follower.id"
        class="ml-1"
      >
        <img :src="Follower.image" width="60" height="60" class="avatar" />
      </router-link>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    Followers: {
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