<style lang="scss">
.image-msg {
  img {
    width: 100%;
    height: auto;
    object-fit: cover;
  }
}
</style>

<template>
  <div class="image-msg">
    <img
      v-for="(src, index) in item.src"
      :key="index"
      :src="src"
    >
    <p v-html="item.content" />
  </div>
</template>

<script>
export default {
  name: 'ImageMsg',
  components: {},
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  data() {
    return {}
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {},
  methods: {}
}
</script>
