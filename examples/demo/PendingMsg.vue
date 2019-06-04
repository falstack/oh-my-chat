<style lang="scss">
</style>

<template>
  <div class="pending-message">
    pending message
    <ul>
      <li
        v-for="(user, index) in users"
        :key="index"
        v-text="user"
      />
    </ul>
    <div>
      <button @click="resolve">
        resolve
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PendingMsg',
  props: {
    item: {
      type: Object,
      required: true
    },
    next: {
      required: true,
      validator: val => typeof val === 'object'
    }
  },
  data() {
    return {
      users: []
    }
  },
  computed: {},
  watch: {
    next() {
      this.next && this.next.then(this.handleNext)
    }
  },
  methods: {
    handleNext(data) {
      this.users.push(data.user.nickname)
    },
    resolve() {
      this.$emit('resolve')
    }
  }
}
</script>
