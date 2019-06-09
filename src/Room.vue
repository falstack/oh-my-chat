<style lang="scss">
.chat-room {
  .msg-fade {
    &-enter-active {
      transition: all 0.3s ease;
    }

    &-enter,
    &-leave-to {
      opacity: 0;
      transform: translateY(10px);
    }
  }
}
</style>

<template>
  <transition-group tag="div" name="msg-fade" class="chat-room">
    <template v-for="(msg, index) in list">
      <Bubble
        v-if="msg.isBubble"
        :key="`${index}-${msg.id}`"
        :color="msg.color"
        :status="msg.status"
        :loading="msg.loading"
        :float="msg.float"
      >
        <component
          :is="avatarComponent"
          v-if="avatarComponent && msg.item.user"
          slot="avatar"
          :item="msg.item.user"
        />
        <component
          :is="msg.header"
          v-if="msg.header && msg.item.user"
          slot="header"
          :item="msg.item.user"
        />
        <component
          :is="loadingComponent"
          v-if="loadingComponent"
          slot="loading"
        />
        <component
          :is="msg.message"
          :key="msg.id"
          :item="msg.item"
          :next="msg.next"
          @loaded="_handleMsgLoaded(index)"
          @resolve="_handleMsgResolve(msg.id)"
        />
        <component
          :is="msg.footer"
          v-if="msg.footer"
          slot="footer"
          :item="msg.item"
        />
      </Bubble>
      <component :is="msg.message" v-else :key="msg.id" :item="msg.item" />
    </template>
  </transition-group>
</template>

<script>
import Bubble from './Bubble'
import TextMsg from './TextMsg'

export default {
  name: 'ChatRoom',
  components: {
    Bubble
  },
  props: {
    avatarComponent: {
      type: Object,
      default: null
    },
    loadingComponent: {
      type: Object,
      default: null
    },
    messageComponents: {
      type: Object,
      default: () => {}
    },
    widgetComponents: {
      type: Object,
      default: () => {}
    },
    headerComponents: {
      type: Object,
      default: () => {}
    },
    footerComponents: {
      type: Object,
      default: () => {}
    },
    colors: {
      type: Array,
      default: () => [
        { bg: '#12b7f5', text: '#fff' },
        { bg: '#ff8eb3', text: '#fff' }
      ]
    }
  },
  data() {
    return {
      list: [],
      last_pending_id: 0,
      resolver: null
    }
  },
  computed: {
    msgTypes() {
      return Object.assign(
        {
          'text-msg': TextMsg
        },
        this.messageComponents
      )
    }
  },
  methods: {
    addMessage(params, insertToAfter = true) {
      const type = params.type || 'text-msg'
      const id =
        params.id ||
        Math.random()
          .toString(10)
          .substring(3, 6)
      const footer = params.footer || ''
      const header = params.header || ''
      const loading = params.loading || false
      const isPromise = params.pending || false
      const data = {
        id,
        isBubble: true,
        message: this.msgTypes[type],
        header: header ? this.headerComponents[header] : null,
        footer: footer ? this.footerComponents[footer] : null,
        float: params.float || 'left',
        color: this._computeBubbleColor(params),
        loading,
        status: params.status || '',
        item: params.data,
        next: null
      }
      insertToAfter ? this.list.push(data) : this.list.unshift(data)
      if (!loading && !isPromise) {
        this._handleResolve(params.data)
      }
      if (isPromise) {
        this.last_pending_id = id
        this._setResolve()
      }
    },
    addWidget(params, insertToAfter = true) {
      const type = params.type || 'divide-line'
      const id =
        params.id ||
        Math.random()
          .toString(10)
          .substring(3, 6)
      const data = {
        id,
        isBubble: false,
        message: this.widgetComponents[type],
        item: params.data
      }
      insertToAfter ? this.list.push(data) : this.list.unshift(data)
    },
    updateMessage(id, obj) {
      for (let i = this.list.length - 1; i >= 0; i--) {
        if (this.list[i].id === id) {
          Object.keys(obj).forEach(key => {
            this.list[i][key] = obj[key]
          })
          break
        }
      }
    },
    _setResolve() {
      setTimeout(() => {
        if (!this.last_pending_id) {
          return
        }
        this.list.forEach((item, index) => {
          if (item.id === this.last_pending_id) {
            this.list[index].next = new Promise(resolve => {
              this.resolver = resolve
            })
          }
        })
      }, 0)
    },
    _handleResolve(data) {
      if (!this.last_pending_id) {
        return
      }
      this.resolver(data)
      this._setResolve()
    },
    _handleMsgLoaded(index) {
      this.list[index].loading = false
      if (this.list[index].next) {
        return
      }
      this._handleResolve(this.list[index].item)
    },
    _handleMsgResolve(id) {
      if (this.last_pending_id !== id) {
        return
      }
      this.last_pending_id = 0
      this.resolver = null
    },
    _computeBubbleColor(item) {
      if (item.color) {
        return item.color
      }
      if (item.data && item.data.user && item.data.user.id) {
        return this.colors[item.data.user.id % this.colors.length]
      }
      return this.colors[item.id % this.colors.length]
    }
  }
}
</script>
