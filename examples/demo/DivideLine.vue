<style lang="scss">
.divide-line {
  background-color: #dcdfe6;
  position: relative;
  display: block;
  height: 1px;
  width: 100%;
  margin: 24px 0;

  .text {
    position: absolute;
    left: 50%;
    padding: 0 20px;
    color: #303133;
    background-color: #fff;
    font-size: 14px;
    transform: translateX(-50%) translateY(-50%);
  }
}
</style>

<template>
  <div class="divide-line">
    <div class="text">
      {{ item.content }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'DivideLine',
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
