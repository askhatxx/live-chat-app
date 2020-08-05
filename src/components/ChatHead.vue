<template>
  <div class="chat-box__head">
    <div class="chat-box__title">LiveChat</div>
    <button 
      @click="$emit('open-chat')"
      class="livechat-btn chat-box__btn-close"
      title="Close"
    >
      <i class="livechat-icon-close livechat-icon-close_head"></i>
    </button>
  </div>
</template>

<style scoped>
.chat-box__head {
  background: #14957B;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  color: #ffffff;
  box-shadow: 0 0 10px rgba(0, 0, 0, .2);
  z-index: 999;
}
.chat-box__btn-close {
  background: none;
  width: 20px;
  height: 20px;
}
</style>>