# oh-my-chat

### install
```sh
yarn add oh-my-chat
// or
npm i oh-my-chat
```

### usage
```javascript
import Vue from 'vue'
import ChatRoom from 'oh-my-chat'
import 'oh-my-chat/dist/oh-my-chat.css'

Vue.component(ChatRoom.name, ChatRoom)
```

```vue
<template>
  <ChatRoom
    ref="room"
    :messages="loadMessageList"
    :avatar-component="avatarComp"
    :message-components="messageComps"
    :header-components="headerComps"
    :footer-components="footerComps"
    :widget-components="widgetComps"
  />
</template>
```

```javascript
export default {
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
      }
  }
}
```

### props
| name | type | required | meaning |
| ---- | ---- | -------- | ------- |
| messages | 消息 array | N | 历史消息填充 |
| avatarComponent | vue-component | N | 头像组件，如果定义了，并且 data 中有 user，就会给展示头像组件 |
| loadingComponent | vue-component | N | loading 组件，当内容是异步展示的时候，会展示 loading，不填则使用默认的 loading |
| messageComponents | <Object>vue-components | N | 自定义气泡容器，默认仅支持 text 容器，通过 addMessage 中的 type 来调用不同容器 |
| widgetComponents | <Object>vue-components | N | 自定义挂件，如分割线等，通过 addWidget 方法调用 |
| headerComponents | <Object>vue-components | N | 气泡顶部的组件，如昵称，通过 addMessage 中的 header 来调用 |
| footerComponents | <Object>vue-components | N | 气泡底部的组件，如点赞，通过 addMessage 中的 footer 来调用 |
| colors | <Array>{ bg: '#12b7f5', text: '#fff' } | N | 气泡的颜色，需要定义背景色和文字的颜色，也可以通过 addMessage 中的 color 自定义 |

### tips
1. 使用自定义组件时，会为每个组件传递一个 item 的 props
2. 使用异步的气泡时，会先展示 loading 组件，当异步组件加载完毕后，需要在组件内手动调用`this.$emit('loaded')`就可以消除 loading
3. 当 addMessage 的 pending 是 true 时，当前气可以通过`watch next()`来获取到后面的气泡数据，如：
```javascript
// pending component
export default {
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
```
4. 除非手动调动`this.$emit('resolve')`或出现下一个`pending component`否则 next 会一直触发
5. 基本上以上的 feature 就可以覆盖表单验证，异步问答这种复杂场景了
