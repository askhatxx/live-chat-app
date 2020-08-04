<template>
  <div class="live-chat">
    <button class="btn chat-toggle chat-toggle_new-msg" @click='openChat'>
      <i v-show='isOpen' class="icon-close"></i>
      <svg v-show='!isOpen' class="icon-logo" xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32">
        <g class="icon-logo__figure">
          <path d="M4,28a.84.84,0,0,1-.38-.08A1,1,0,0,1,3,27V8.78A4.89,4.89,0,0,1,8,4H24a4.89,4.89,0,0,1,5,4.78v9.44A4.89,4.89,0,0,1,24,23H9.41l-4.7,4.71A1,1,0,0,1,4,28ZM8,6A2.9,2.9,0,0,0,5,8.78V24.59l3.29-3.3A1,1,0,0,1,9,21H24a2.9,2.9,0,0,0,3-2.78V8.78A2.9,2.9,0,0,0,24,6Z"/>
          <circle cx="16" cy="13.5" r="1.5"/>
          <circle cx="21.5" cy="13.5" r="1.5"/>
          <circle cx="10.5" cy="13.5" r="1.5"/>
        </g>
        <g>
          <rect height="32" width="32"/>
        </g>
      </svg>
    </button>
    <div class="chat-box" :class='{"chat-box_open": isOpen}'>
      <ChatHead @open-chat='openChat'/>
      <ChatList/>
      <ChatFooter/>
    </div>
  </div>
</template>

<script>
import ChatHead from '@/components/ChatHead'
import ChatList from '@/components/ChatList'
import ChatFooter from '@/components/ChatFooter'

export default {
  data() {
    return {
      isOpen: false
    }
  },
  components: {
    ChatHead, ChatList, ChatFooter
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

.live-chat {
  position: fixed;
  right: 10px;
  bottom: 10px;
  font-size: .9rem;
}

.btn {
  font-family: inherit;
  border: none;
  outline: none;
  user-select: none;
  cursor: pointer;
}

.chat-toggle {
  background: #14957B;
  position: relative;
  width: 50px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  border-radius: 50%;
  transition: 2.3s;
}
.chat-toggle_new-msg::after {
  content: '';
  position: absolute;
  top: 2px;
  right: 2px;
  background: #FF3D1B;
  width: 10px;
  height: 10px;
  border-radius: 50%;
}
.chat-toggle_new-msg {
  animation-duration: 1s;
  animation-name: anim-new-msg;
}
@keyframes anim-new-msg {
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

.icon-logo {
  fill: none;
  width: 30px;
  height: 30px;
}
.icon-logo__figure {
  fill: #ffffff;
}

.icon-close {
  position: relative;
  display: inline-block;
  width: 20px;
  height: 20px;
}
.icon-close::before,
.icon-close::after {
  content: '';
  background: #ffffff;
  display: block;
  width: 100%;
  height: 10%;
  border-radius: 4px;
  position: absolute;
  top: 50%;
}
.icon-close::before {
  transform: translateY(-50%) rotate(45deg);
}
.icon-close::after {
  transform: translateY(-50%) rotate(-45deg);
}

/************ CHAT ************/

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
