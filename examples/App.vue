<template>
  <section class="container">
    <div class="room-wrap">
      <Room
        ref="room"
        :avatar-component="avatarComp"
        :message-components="messageComps"
        :header-components="headerComps"
        :footer-components="footerComps"
        :widget-components="widgetComps"
      />
    </div>
    <div class="control">
      <button @click="handleAddBubble('text-msg')">
        add text
      </button>
      <button @click="handleAddBubble('image-msg')">
        add image
      </button>
      <button @click="handleAddBubble('loading-msg')">
        add loading
      </button>
      <button @click="handleAddBubble('pending-msg')">
        add pending
      </button>
      <button @click="handleAddWeight('divide-line')">
        add line
      </button>
    </div>
  </section>
</template>

<script>
import Room from '../src/Room.vue'
import LoadingMsg from './demo/LoadingMsg'
import ImageMsg from './demo/ImageMsg'
import PendingMsg from './demo/PendingMsg'
import Avatar from './demo/Avatar'
import Nickname from './demo/Nickname'
import CreatedAt from './demo/CreatedAt'
import DivideLine from './demo/DivideLine'
import faker from 'faker'

export default {
  components: {
    Room
  },
  computed: {
    messageComps() {
      return {
        'loading-msg': LoadingMsg,
        'image-msg': ImageMsg,
        'pending-msg': PendingMsg
      }
    },
    widgetComps() {
      return {
        'divide-line': DivideLine
      }
    },
    headerComps() {
      return {
        'nickname': Nickname
      }
    },
    footerComps() {
      return {
        'created-at': CreatedAt
      }
    },
    avatarComp() {
      return Avatar
    }
  },
  methods: {
    handleAddBubble(type) {
      const images = new Array(Math.floor(Math.random() * 9)).fill('')
      images.forEach((item, index) => {
        images[index] = faker.image.image()
      })
      this.$refs.room.addMessage({
        type,
        float: type === 'text-msg' ? 'left' : 'right',
        id: Math.random().toString(10).substring(3, 6),
        loading: type === 'loading-msg',
        pending: type === 'pending-msg',
        header: 'nickname',
        footer: 'created-at',
        status: 'success',
        data: {
          content: this.randomContent(),
          src: images,
          created_at: faker.date.recent(),
          user: {
            id: Math.random().toString(10).substring(3, 6),
            avatar: faker.image.avatar(),
            nickname: faker.name.findName()
          }
        }
      })
    },
    handleAddWeight(type) {
      this.$refs.room.addWidget({
        type,
        data: {
          content: '哈哈哈'
        }
      })
    },
    randomContent() {
      const arr = [
        faker.lorem.paragraphs,
        faker.lorem.paragraph,
        faker.lorem.word,
        faker.lorem.words,
        faker.lorem.sentence,
        faker.lorem.sentences,
        faker.lorem.text,
        faker.lorem.lines
      ]

      return arr[Math.floor(Math.random() * arr.length)]()
    }
  }
}
</script>

<style lang="scss">
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;

  .room-wrap {
    width: 560px;
    height: 500px;
    overflow: auto;
    padding: 20px;
    border: 1px solid #47494e;
  }

  .msg-status {
    display: block;
    width: 14px;
    height: 14px;
    border-radius: 50%;
    background-color: lawngreen;
  }
}
</style>
