<template>
  <div class="admin-panel__open-chat">
    <div v-if="openChatGet.success">
      <div v-for="msg in openChatGet.chat" :key="msg.id">{{ msg.text + ' - ' + msg.read }}</div>
      <input v-model="text" placeholder="Text"/>
      <button @click="$emit('send-msg', text)">Send text</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text: ''
    }
  },
  props: ['openChatGet']
}
</script>

<style scoped>
.admin-panel__open-chat {
  flex-grow: 1;
}
</style>