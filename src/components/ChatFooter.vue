<template>
  <div class="chat-box__footer">
    <div 
      ref="textInput"
      @keydown.enter.prevent="sendText"
      class="chat-box__input"
      contenteditable="true"
      tabindex="0"
      placeholder="Write a message"
    ></div>
    <div class="chat-box__options">
      <button 
        @click="sendText"
        class="livechat-btn chat-box__btn-send"
        title="Send"
      >
        <svg class="icon-send" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
          <path d="M2.01 21L23 12 2.01 3 2 10l15 2-15 2z"/>
        </svg>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    sendText() {
      if (this.$refs.textInput.innerText.trim().length > 0) {
        this.$emit('send-msg', this.$refs.textInput.innerText)
        this.$refs.textInput.innerText = ''
      }
    }
  }
}
</script>

<style scoped>
.chat-box__footer {
  display: flex;
  align-items: center;
  background: #ffffff;
  box-shadow: 0 0 10px rgba(0, 0, 0, .1);
  z-index: 999;
}
.chat-box__input {
  flex-grow: 1;
  outline: none;
  min-height: 40px;
  max-height: 100px;
  overflow-x: hidden;
  overflow-y: auto;
  white-space: pre-wrap;
  word-wrap: break-word;
  cursor: text;
  padding: 10px;
  line-height: 1.4;
}
.chat-box__input:empty::before {
  content: attr(placeholder);
  color: #cccccc;
}
.chat-box__options {
  padding: 10px;
}
.chat-box__btn-send {
  background: none;
  display: flex;
}
.icon-send {
  fill: #333333;
  width: 20px;
  height: 20px;
}
</style>>