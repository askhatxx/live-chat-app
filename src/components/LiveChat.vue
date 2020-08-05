<template>
  <div class="live-chat">
    <ChatToggle 
      @open-chat="openChat"
      :isOpen="isOpen"
      :isNewMsg="isNewMsg"
    />
    <div class="chat-box" :class="{'chat-box_open': isOpen}">
      <ChatHead @open-chat="openChat"/>
      <ChatList :chat="chat"/>
      <ChatFooter/>
    </div>
  </div>
</template>

<script>
import ChatToggle from '@/components/ChatToggle'
import ChatHead from '@/components/ChatHead'
import ChatList from '@/components/ChatList'
import ChatFooter from '@/components/ChatFooter'

export default {
  data() {
    return {
      isOpen: false,
      isNewMsg: true,
      chat: [
        { text: 'Hello', date: '24.07.20 15:40', id: '01', author: 'user' },
        { text: 'Hi!', date: '24.07.20 16:35', id: '02', author: 'admin' },
        { text: 'Lorem ipsum dolor, sit amet consectetur adipisicing elit.', date: '24.07.20 15:40', id: '03', author: 'user' },
        { text: 'Lorem ipsum dolor, sit amet consectetur adipisicing elit.', date: '24.07.20 16:35', id: '04', author: 'admin' },
        { text: 'Lorem ipsum dolor', date: '24.07.20 15:40', id: '05', author: 'user' },
        { text: 'Lorem ipsumdolorsitametconsecteturadipisicingelit.', date: '24.07.20 16:35', id: '06', author: 'admin' },
      ]
    }
  },
  components: {
    ChatToggle, ChatHead, ChatList, ChatFooter
  },
  methods: {
    openChat() {
      this.isOpen = !this.isOpen
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
