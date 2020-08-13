<template>
  <div class="admin-panel__open-chat">
    <div v-if="openChatGet.success" class="admin-panel__chat-box">
      <div class="admin-panel__head-chat">
        <button 
          @click="deleteChat(openChatGet.id)"
          class="livechat-btn admin-panel__btn-del-chat"
          title="Delete chat"
        >
          Delete chat
        </button>
      </div>
      <ChatList :chat="openChatGet.chat" :showRead="true"/>
      <ChatFooter @send-msg="sendText"/>
    </div>
    <div v-else class="admin-panel__select-chat">Please select chat</div>
  </div>
</template>

<script>
import ChatList from '@/components/ChatList'
import ChatFooter from '@/components/ChatFooter'

export default {
  props: ['openChatGet'],
  components: {
    ChatList, ChatFooter
  },
  methods: {
    sendText(text) {
      this.$emit('send-msg', text)
    },
    deleteChat(id) {
      this.$emit('delete-chat', id)
    }
  }
}
</script>

<style scoped>
.admin-panel__open-chat {
  flex: 1 0 66.66%;
  padding: 10px;
}
@media (max-width: 500px) {
  .admin-panel__open-chat {
    flex: 0 0 100%;
  }
}
.admin-panel__chat-box {
  background: #f2f2f2;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  height: calc(100vh - 100px);
  overflow: hidden;
  box-shadow: 0 5px 10px rgba(0, 0, 0, .2);
}
.admin-panel__head-chat {
  background: #14957B;
  padding: 8px;
  color: #ffffff;
  box-shadow: 0 0 10px rgba(0, 0, 0, .2);
  z-index: 999;
  text-align: right;
}
.admin-panel__btn-del-chat {
  background: #3db29a;
  color: #ffffff;
  padding: 3px 8px;
  border-radius: 6px;
  font-size: .9rem;
  transition: .1s;
}
.admin-panel__btn-del-chat:hover {
  background: #24a188;
}
.admin-panel__select-chat {
  height: 100%;
  background: #f2f2f2;
  border-radius: 10px;
  padding: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>