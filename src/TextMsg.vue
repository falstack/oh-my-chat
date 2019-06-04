<template>
  <div class="text-msg" v-html="item.content" />
</template>

<script>
export default {
  name: 'TextMsg',
  props: {
    item: {
      type: Object,
      required: true
    }
  }
}
</script>
