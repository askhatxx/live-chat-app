<template>
  <div class="live-chat">
    <ChatToggle 
      @render-chat="renderChat"
      @open-chat="openChat"
      :isOpen="isOpen"
      :isNewMsg="isNewMsg"
    />
    <div class="chat-box" :class="{'chat-box_open': isOpen}">
      <template v-if="isRender">
        <ChatHead @open-chat="openChat"/>
        <ChatList :chat="chat"/>
        <ChatFooter @send-msg="sendMsg"/>
      </template>
    </div>
  </div>
</template>

<script>
import ChatToggle from '@/components/ChatToggle'
import ChatHead from '@/components/ChatHead'
import ChatList from '@/components/ChatList'
import ChatFooter from '@/components/ChatFooter'
import { db } from '@/base'

export default {
  data() {
    return {
      isOpen: false,
      isNewMsg: true,
      isRender: false,
      chat: [
        // { text: 'Hello', date: 1595241029985, id: 1595241029985, author: 'user', read: true },
        // { text: 'Hi!', date: 1595241129985, id: 1595241129985, author: 'admin', read: true },
        // { text: 'Lorem ipsum dolor, sit amet consectetur adipisicing elit.', date: 1595241329985, id: 1595241329985, author: 'user', read: true },
        // { text: 'Lorem ipsum dolor, sit amet consectetur adipisicing elit.', date: 1595241529985, id: 1595241529985, author: 'admin', read: true },
        // { text: 'Lorem ipsum dolor', date: 1595241729985, id: 1595241729985, author: 'user', read: true },
        // { text: 'Lorem ipsumdolorsitametconsecteturadipisicingelit.', date: 1595241929985, id: 1595241929985, author: 'admin', read: true },
      ]
    }
  },
  components: {
    ChatToggle, ChatHead, ChatList, ChatFooter
  },
  mounted() {
    //const idChat = localStorage.getItem('id-chat')
    this.getMsg()
  },
  methods: {
    openChat() {
      this.isOpen = !this.isOpen
    },
    renderChat() {
      this.isRender = true
    },
    sendMsg(text) {
      db.collection('chat-temp')
        .doc('F9QbWJDZBTz95NZynurx')
        .update({chat: [...this.chat, { text: text, date: Date.now(), id: Date.now(), author: 'user', read: true }]})
        .then(() => {
          console.log('Update doc --->')
        })
        .catch((error) => {
          console.log('db error', error)
        })
    },
    getMsg() {
      db.collection('chat-temp')
        .doc('F9QbWJDZBTz95NZynurx')
        .get()
        .then(doc => {
          console.log('Get doc --->', doc)
          console.log('Get doc.exists --->', doc.exists)
          console.log('Get doc.data --->', doc.data())
          this.chat = doc.data().chat
        })
        .catch((error) => {
          console.log('db error', error)
        })
    }
  }
}
</script>

<style>
.livechat-btn {
  font-family: inherit;
  border: none;
  outline: none;
  user-select: none;
  cursor: pointer;
}

.livechat-icon-close {
  position: relative;
  display: inline-block;
  width: 20px;
  height: 20px;
}
.livechat-icon-close::before,
.livechat-icon-close::after {
  content: '';
  background: #ffffff;
  display: block;
  width: 100%;
  height: 10%;
  border-radius: 4px;
  position: absolute;
  top: 50%;
}
.livechat-icon-close::before {
  transform: translateY(-50%) rotate(45deg);
}
.livechat-icon-close::after {
  transform: translateY(-50%) rotate(-45deg);
}
.livechat-icon-close_head {
  width: 16px;
  height: 16px;
  transition: .1s;
  opacity: .7;
}
.livechat-icon-close_head:hover {
  opacity: 1;
}
</style>

<style scoped>
.live-chat {
  position: fixed;
  right: 10px;
  bottom: 10px;
  font-size: .9rem;
}

.chat-box {
  background: #f2f2f2;
  border-radius: 10px;
  box-shadow: 0 5px 30px rgba(0, 0, 0, .2);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  width: calc(100vw - 20px);
  max-width: 300px;
  height: calc(100vh - 20px - 60px);
  max-height: 400px;
  position: absolute;
  right: 0;
  bottom: 60px;
  transition: .3s;
  transform-origin: right bottom;
  opacity: 0;
  transform: scale(0);
}
.chat-box_open {
  opacity: 1;
  transform: scale(1);
}
</style>
