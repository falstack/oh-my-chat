<style lang="scss">
</style>

<template>
  <div class="loading-msg">
    {{ item }}
  </div>
</template>

<script>
export default {
  name: 'LoadingMsg',
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
  mounted() {
    this.getData()
  },
  methods: {
    getData() {
      setTimeout(() => {
        this.$emit('loaded')
      }, 3000)
    }
  }
}
</script>
