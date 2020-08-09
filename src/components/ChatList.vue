<template>
  <div ref="chatList" class="chat-box__list">
    <div 
      v-for="msg in chat" 
      :key="msg.id" 
      class="chat-box__msg-block" 
      :class="{'chat-box__msg-block_sent': msg.author === 'user'}"
    >
      <div class="chat-box__msg-text">
        <div>{{ msg.text }}</div>
        <div class="chat-box__msg-date">{{ msg.date | formatDate }}</div>
      </div>
    </div>
  </div>
</template>

<script>
const formatDateAddZero = num => {
  if (num < 10) return '0' + num
  return num
}

export default {
  props: ['chat'],
  mounted() {
    console.log('ChatList mounted')
    this.scrollToBottom()
  },
  updated() {
    console.log('ChatList updated')
    this.scrollToBottom()
  },
  methods: {
    scrollToBottom() {
      this.$refs.chatList.scrollTop = this.$refs.chatList.scrollHeight
    }
  },
  filters: {
    formatDate(value) {
      const date = new Date(+value)
      const day = formatDateAddZero(date.getDate())
      const month = formatDateAddZero(date.getMonth() + 1)
      const year = date.getFullYear()
      const hours = formatDateAddZero(date.getHours())
      const minutes = formatDateAddZero(date.getMinutes())
      return `${day}.${month}.${year} ${hours}:${minutes}`
    }
  }
}
</script>

<style scoped>
.chat-box__list {
  overflow-x: hidden;
  overflow-y: auto;
  flex-grow: 1;
}
.chat-box__msg-block {
  padding: 10px;
  display: flex;
}
.chat-box__msg-block_sent {
  justify-content: flex-end;
}
.chat-box__msg-text {
  white-space: pre-wrap;
  word-wrap: break-word;
  padding: 10px;
  max-width: 80%;
  color: #555555;
  background: #ffffff;
  border-radius: 10px;
  border-bottom-left-radius: 0;
}
.chat-box__msg-block_sent .chat-box__msg-text {
  color: #ffffff;
  background: #14957B;
  border-radius: 10px;
  border-bottom-right-radius: 0;
}
.chat-box__msg-date {
  display: inline-block;
  background: rgba(0, 0, 0, .1);
  opacity: .7;
  margin-top: 5px;
  padding: 1px 5px;
  border-radius: 5px;
  font-size: .8rem;
}
</style>>