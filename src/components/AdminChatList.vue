<template>
  <div class="admin-panel__chat-list">
    <div v-if="loadingDB" class="admin-panel__loading-db">
      <Loading/>
    </div>
    <ul v-else-if="chatList.length" class="admin-panel__chat-ul">
      <li 
        v-for="chat in chatList"
        :key="chat.id"
        @click="$emit('open-chat-id-set', chat.id)"
        class="admin-panel__chat-li"
        :class="{'admin-panel__chat-li_active': chat.id === openChatId, 'admin-panel__chat-li_new-msg': checkNewMsg(chat)}"
      >
        {{ chat.id }}
      </li>
    </ul>
    <div v-else class="admin-panel__no-chat">No chats</div>
  </div>
</template>

<script>
import Loading from '@/components/Loading'

export default {
  props: ['loadingDB', 'chatList', 'openChatId'],
  components: {
    Loading
  },
  methods: {
    checkNewMsg(chat) {
      if (chat.id === this.openChatId) return false
      return chat.chat.some(item => item.author === 'user' && !item.read)
    }
  }
}
</script>

<style scoped>
.admin-panel__chat-list {
  flex: 0 0 33.33%;
  max-width: 33.33%;
  padding: 10px;
}
@media (max-width: 500px) {
  .admin-panel__chat-list {
    flex: 0 0 100%;
    max-width: 100%;
  }
}
.admin-panel__chat-ul {
  list-style: none;
  max-height: calc(100vh - 100px);
  overflow-x: hidden;
  overflow-y: auto;
  border-radius: 10px;
}
.admin-panel__chat-li {
  display: block;
  position: relative;
  padding: 10px;
  padding-right: 15px;
  background: #14957B;
  border-bottom: 1px solid #3db29a;
  color: #ffffff;
  word-wrap: break-word;
  transition: .1s;
  cursor: pointer;
}
.admin-panel__chat-li:hover {
  background: #24a188;
}
.admin-panel__chat-li:first-child {
  border-radius: 10px 10px 0 0;
}
.admin-panel__chat-li:last-child {
  border-radius: 0 0 10px 10px;
  border-bottom: none;
}
.admin-panel__chat-li_active {
  background: #3db29a;
}
.admin-panel__chat-li_new-msg::after {
  content: '';
  position: absolute;
  top: 50%;
  right: 5px;
  transform: translateY(-50%);
  background: #FF3D1B;
  width: 10px;
  height: 10px;
  border-radius: 50%;
}
.admin-panel__loading-db {
  background: #14957B;
  padding: 10px;
  border-radius: 10px;
  text-align: center;
}
.admin-panel__no-chat {
  background: #14957B;
  padding: 20px 10px;
  color: #ffffff;
  border-radius: 10px;
  text-align: center;
}
</style>