<template>
  <div class="widget-chat" :class='{open: isOpen}'>
    <button class="btn-chat-open new-msg" @click='openChat'>Open</button>
    <div class="chat">
      <div class="chat-head">
        <div class="chat-head-title">Chat...</div>
        <button class="chat-head-close" @click='openChat'>×</button>
      </div>
      <div class="chat-list">
        <div class="chat-list-item">Hi!</div>
        <div class="chat-list-item">Hello</div>
        <div class="chat-list-item">Lorem ipsum dolor, sit amet consectetur adipisicing elit.</div>
        <div class="chat-list-item">Hello</div>
        <div class="chat-list-item">Hello</div>
        <div class="chat-list-item">Lorem ipsumdolorsitametconsecteturadipisicingelit.</div>
        <div class="chat-list-item">Hello</div>
        <div class="chat-list-item">Hello</div>
        <div class="chat-list-item">Lorem ipsum dolor.</div>
        <div class="chat-list-item">Hi!</div>
        <div class="chat-list-item">Hi!</div>
        <div class="chat-list-item">Lorem ipsum dolor.</div>
        <div class="chat-list-item">Hello</div>
        <div class="chat-list-item">Lorem ipsum dolor.</div>
        <div class="chat-list-item">Lorem ipsum dolor, sit amet consectetur adipisicing elit.</div>
      </div>
      <div class="chat-msg">
        <div class="chat-msg-input" contenteditable="true" tabindex="0" placeholder="Write a message"></div>
        <div class="chat-msg-options">
          <button class="chat-msg-options-send">Send</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isOpen: false
    }
  },
  methods: {
    openChat() {
      this.isOpen = !this.isOpen
    }
  }
}
</script>

<style scoped>
/************ WIDGET ************/

.widget-chat {
  position: fixed;
  right: 10px;
  bottom: 10px;
}
.widget-chat button {
  font-family: inherit;
  border: none;
  outline: none;
  user-select: none;
  cursor: pointer;
}
.btn-chat-open {
  background: orangered;
  position: relative;
  width: 50px;
  height: 50px;
  line-height: 50px;
  font-size: 1rem;
  text-align: center;
  color: white;
  border-radius: 50%;
  transition: 2.3s;
}
.btn-chat-open.new-msg::after {
  content: '';
  position: absolute;
  top: 2px;
  right: 2px;
  background: blue;
  width: 10px;
  height: 10px;
  border-radius: 50%;
}
.btn-chat-open.new-msg {
  animation-duration: 1s;
  animation-name: anim-1;
}
@keyframes anim-1 {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.5);
    background: darkred;
  }
  100% {
    transform: scale(1);
  }
}

/************ CHAT ************/

.chat {
  background: orange;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  width: calc(100vw - 20px);
  max-width: 300px;
  height: calc(100vh - 20px - 60px);
  max-height: 400px;
  padding: 10px;
  position: absolute;
  right: 0;
  bottom: 60px;
  transition: .3s;
  transform-origin: right bottom;
  opacity: 0;
  transform: scale(0);
}
.open .chat {
  opacity: 1;
  transform: scale(1);
}

/************ CHAT HEAD ************/

.chat-head {
  display: flex;
  justify-content: space-between;
}
.chat-head-close {
  background: orangered;
  width: 30px;
  height: 30px;
  line-height: 30px;
  font-size: 1rem;
  text-align: center;
  color: white;
  border-radius: 50%;
}

/************ CHAT LIST ************/

.chat-list {
  overflow-x: hidden;
  overflow-y: auto;
  flex-grow: 1;
}
.chat-list-item {
  white-space: pre-wrap;
  word-wrap: break-word;
}

/************ CHAT MSG ************/

.chat-msg {
  display: flex;
}
.chat-msg-input {
  flex-grow: 1;
  outline: none;
  min-height: 20px;
  max-height: 100px;
  overflow-x: hidden;
  overflow-y: auto;
  white-space: pre-wrap;
  word-wrap: break-word;
  cursor: text;
}
.chat-msg-input:empty::before {
  content: attr(placeholder);
  color: rgba(255, 255, 255, .5);
}
</style>
