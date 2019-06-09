<style lang="scss">
.msg-box {
  min-height: 30px;
  margin: 10px 0;

  &:after {
    content: '';
    clear: both;
    display: table;
  }

  &:before {
    content: '';
    clear: both;
    display: table;
  }

  &-left {
    .msg-avatar {
      float: left;
    }

    .msg-body {
      text-align: left;
    }

    .msg-status {
      right: -20px;
    }
  }

  &-right {
    .msg-avatar {
      float: right;
    }

    .msg-body {
      text-align: right;
    }

    .msg-status {
      left: -20px;
    }
  }

  .msg-body {
    overflow: hidden;

    .msg-bubble {
      position: relative;
      padding: 7px 13px;
      border-radius: 15px;
      max-width: 55%;
      display: inline-block;
      text-align: left;
      word-break: break-word !important;
      word-break: break-all;
      font-size: 14px;
      line-height: 1.6;

      .msg-status {
        position: absolute;
        top: 11px;
      }
    }
  }
}
</style>

<template>
  <div :class="`msg-box-${float}`" class="msg-box">
    <div class="msg-avatar">
      <slot name="avatar" />
    </div>
    <div class="msg-body">
      <slot name="header" />
      <div :style="wrapperStyle" class="msg-bubble">
        <slot v-if="loading" name="loading">
          <loading-msg />
        </slot>
        <span v-show="!loading">
          <slot />
          <i v-if="status" class="msg-status" :class="`msg-status-${status}`" />
        </span>
      </div>
      <slot name="footer" />
    </div>
  </div>
</template>

<script>
import LoadingMsg from './Loading'

export default {
  name: 'MsgBubble',
  components: {
    LoadingMsg
  },
  props: {
    color: {
      type: Object,
      required: true
    },
    float: {
      type: String,
      required: true,
      validator: val => ~['left', 'right'].indexOf(val)
    },
    loading: {
      type: Boolean,
      required: true
    },
    status: {
      type: String,
      required: true
    }
  },
  computed: {
    wrapperStyle() {
      return {
        backgroundColor: this.color.bg,
        color: this.color.text
      }
    }
  }
}
</script>
