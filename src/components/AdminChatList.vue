<template>
  <div class="admin-panel__chat-list">
    <ul class="admin-panel__chat-ul">
      <li 
        v-for="chat in chatList"
        :key="chat.id"
        @click="$emit('open-chat-id-set', chat.id)"
        class="admin-panel__chat-li"
        :class="{'admin-panel__chat-li_active': chat.id === openChatId}"
      >
        {{ chat.id }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['chatList', 'openChatId']
}
</script>

<style scoped>
.admin-panel__chat-list {
  width: 100%;
  max-width: 300px;
  padding: 10px;
}
.admin-panel__chat-ul {
  list-style: none;
}
.admin-panel__chat-li {
  display: block;
  padding: 10px;
  background: #14957B;
  border-bottom: 1px solid #4AB9A3;
  color: #ffffff;
  cursor: pointer;
}
.admin-panel__chat-li:first-child {
  border-radius: 10px 10px 0 0;
}
.admin-panel__chat-li:last-child {
  border-radius: 0 0 10px 10px;
  border-bottom: none;
}
.admin-panel__chat-li_active {
  background: #4AB9A3;
}
</style>