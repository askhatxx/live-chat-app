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
      <ChatList :chat="chat"/>
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
import { db } from '@/base'

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
    console.log('LiveChat mounted')
    const chatID = localStorage.getItem('chat-id') // 'F9QbWJDZBTz95NZynurx'
    if (chatID) {
      this.chatID = chatID
      this.subscribeDB()
    }
  },
  beforeDestroy() {
    console.log('LiveChat beforeDestroy')
    if (this.unsubscribeDB) this.unsubscribeDB()
  },
  methods: {
    openChat() {
      console.log('openChat')
      this.isOpen = !this.isOpen
      if (this.isOpen) {
        this.isNewMsg = false
        this.allMsgRead()
      }
    },
    renderChat() {
      console.log('renderChat')
      this.isRender = true
    },
    sendMsg(text) {
      console.log('sendMsg')
      const msg = { text: text, date: Date.now(), id: Date.now(), author: 'user', read: true }
      if (this.chatID) {
        db.collection('chat-temp')
          .doc(this.chatID)
          .update({chat: [...this.chat, msg]})
          .then(() => {
            console.log('Update doc --->')
            if (this.infoAlert.show) this.infoAlert.show = false
          })
          .catch(error => {
            console.log('DB error', error)
            this.infoAlert = { show: true, text: 'Data base error', type: 'error' }
          })
      } else {
        db.collection('chat-temp')
          .add({chat: [...this.chat, msg]})
          .then(docRef => {
            console.log('Add doc id', docRef.id)
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
      console.log('allMsgRead check')
      if (this.chat.some(item => !item.read)) {
        console.log('allMsgRead update DB')
        db.collection('chat-temp')
          .doc(this.chatID)
          .update({chat: this.chat.map(item => ({...item, read: true}))})
          .then(() => {
            console.log('Update doc --->')
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
        this.unsubscribeDB = db.collection('chat-temp')
          .doc(this.chatID)
          .onSnapshot(doc => {
            console.log('Listen doc ----->', doc)
            console.log('Listen doc.id ----->', doc.id)
            console.log('Listen doc.exists ----->', doc.exists)
            if (doc.exists) {
              console.log('Listen doc.data', doc.data())
              this.chat = doc.data().chat
              if (!this.isOpen && this.chat.some(item => !item.read)) {
                this.isNewMsg = true
              } else if (this.isOpen) {
                this.allMsgRead()
              }
            } else {
              console.log('Listen doc.exists false ----->')
              this.chatID = null
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
