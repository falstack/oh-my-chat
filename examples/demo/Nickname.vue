<style lang="scss">
.demo-nickname {
  font-size: 13px;
}
</style>

<template>
  <div class="demo-nickname">
    {{ item.nickname }}
  </div>
</template>

<script>
export default {
  name: 'Nickname',
  props: {
    item: {
      type: Object,
      required: true
    }
  }
}
</script>
