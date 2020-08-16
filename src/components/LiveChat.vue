<template>
  <div class="live-chat">
    <ChatToggle 
      @render-chat="renderChat"
      @open-chat="openChat"
      :isOpen="isOpen"
      :isNewMsg="isNewMsg"
    />
    <div v-if="isRender" class="chat-box" :class="{'chat-box_open': isOpen}">
      <ChatHead @open-chat="openChat"/>
      <ChatList :chat="chat" :showRead="false"/>
      <ChatInfo v-if="infoAlert.show" :info="infoAlert"/>
      <ChatFooter @send-msg="sendMsg"/>
    </div>
  </div>
</template>

<script>
import ChatToggle from '@/components/ChatToggle'
import ChatHead from '@/components/ChatHead'
import ChatList from '@/components/ChatList'
import ChatInfo from '@/components/ChatInfo'
import ChatFooter from '@/components/ChatFooter'
import { dbCollectionChat } from '@/base'

export default {
  data() {
    return {
      isOpen: false,
      isNewMsg: false,
      isRender: false,
      unsubscribeDB: null,
      chatID: null,
      infoAlert: { show: false, text: '', type: '' },
      chat: []
    }
  },
  components: {
    ChatToggle, ChatHead, ChatList, ChatInfo, ChatFooter
  },
  mounted() {
    const chatID = localStorage.getItem('chat-id')
    if (chatID) {
      this.chatID = chatID
      this.subscribeDB()
    }
  },
  beforeDestroy() {
    if (this.unsubscribeDB) this.unsubscribeDB()
  },
  methods: {
    openChat() {
      this.isOpen = !this.isOpen
      if (this.isOpen) {
        this.isNewMsg = false
        this.allMsgRead()
      }
    },
    renderChat() {
      this.isRender = true
    },
    sendMsg(text) {
      const msg = { text: text, date: Date.now(), id: Date.now(), author: 'user', read: false }
      if (this.chatID) {
        dbCollectionChat
          .doc(this.chatID)
          .update({chat: [...this.chat, msg]})
          .then(() => {
            if (this.infoAlert.show) this.infoAlert.show = false
          })
          .catch(error => {
            console.log('DB error', error)
            this.infoAlert = { show: true, text: 'Data base error', type: 'error' }
          })
      } else {
        dbCollectionChat
          .add({chat: [...this.chat, msg]})
          .then(docRef => {
            this.chatID = docRef.id
            localStorage.setItem('chat-id', docRef.id)
            this.subscribeDB()
            if (this.infoAlert.show) this.infoAlert.show = false
          })
          .catch(error => {
            console.log('DB error', error)
            this.infoAlert = { show: true, text: 'Data base error', type: 'error' }
          })
      }
    },
    allMsgRead() {
      if (this.chat.some(item => item.author === 'admin' && !item.read)) {
        dbCollectionChat
          .doc(this.chatID)
          .update({chat: this.chat.map(item => {
            if (item.author === 'admin') return {...item, read: true}
            return item
          })})
          .then(() => {
            if (this.infoAlert.show) this.infoAlert.show = false
          })
          .catch(error => {
            console.log('DB error', error)
            this.infoAlert = { show: true, text: 'Data base error', type: 'error' }
          })
      }
    },
    subscribeDB() {
      if (this.chatID) {
        this.unsubscribeDB = dbCollectionChat
          .doc(this.chatID)
          .onSnapshot(doc => {
            if (doc.exists) {
              this.chat = doc.data().chat
              if (!this.isOpen && this.chat.some(item => item.author === 'admin' && !item.read)) {
                this.isNewMsg = true
              } else if (this.isOpen) {
                this.allMsgRead()
              }
            } else {
              this.chat = []
              this.chatID = null
              localStorage.removeItem('chat-id')
            }
          })
      }
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
