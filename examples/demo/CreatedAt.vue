<style lang="scss">
</style>

<template>
  <div>
    <time>
      {{ item.created_at }}
    </time>
  </div>
</template>

<script>
export default {
  name: '',
  components: {},
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {},
  methods: {}
}
</script>
