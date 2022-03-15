<template>
  <div class="card">
    <div class="card-header">
      <strong>{{ Followings | length }}</strong> followerings (追隨者)
    </div>
    <div class="card-body">
      <router-link
        :to="{ name: 'user', params: { id: Following.id } }"
        v-for="Following in Followings"
        :key="Following.id"
        class="ml-1"
      >
        <img :src="Following.image" width="60" height="60" class="avatar" />
      </router-link>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    Followings: {
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