<template>
  <div class="text-msg" v-text="item.content" />
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
