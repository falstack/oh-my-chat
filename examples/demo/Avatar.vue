<style lang="scss">
.demo-avatar {
  width: 50px;
  height: 50px;
  display: block;
  border-radius: 50%;
  object-fit: cover;
}
</style>

<template>
  <img
    class="demo-avatar"
    :src="item.avatar"
    :alt="item.nickname"
  >
</template>

<script>
export default {
  name: 'Avatar',
  props: {
    item: {
      type: Object,
      required: true
    }
  }
}
</script>
