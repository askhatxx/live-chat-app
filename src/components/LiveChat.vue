<template>
  <div class="widget-chat" :class='{open: isOpen}'>
    <button class="btn-chat-open new-msg" @click='openChat'>
      <i v-show='isOpen' class="icon-close"></i>
      <svg v-show='!isOpen' class="icon-logo" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
        <g class="fill">
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
    <div class="chat">
      <div class="chat-head">
        <div class="chat-head-title">LiveChat</div>
        <button class="chat-head-close" @click='openChat' title="Close">
          <i class="icon-close"></i>
        </button>
      </div>
      <div class="chat-list">
        
        <div class="chat-list-item chat-list-sent">
          <div class="chat-list-item-msg">
            <div>Hello</div>
            <div class="chat-list-item-msg-date">24.07.20 15:40</div>
          </div>
        </div>
        <div class="chat-list-item chat-list-received">
          <div class="chat-list-item-msg">
            <div>Hi!</div>
            <div class="chat-list-item-msg-date">24.07.20 15:40</div>
          </div>
        </div>
        <div class="chat-list-item chat-list-sent">
          <div class="chat-list-item-msg">
            <div>Lorem ipsum dolor, sit amet consectetur adipisicing elit.</div>
            <div class="chat-list-item-msg-date">24.07.20 15:40</div>
          </div>
        </div>
        <div class="chat-list-item chat-list-received">
          <div class="chat-list-item-msg">
            <div>Lorem ipsum dolor, sit amet consectetur adipisicing elit.</div>
            <div class="chat-list-item-msg-date">24.07.20 16:35</div>
          </div>
        </div>
        <div class="chat-list-item chat-list-sent">
          <div class="chat-list-item-msg">
            <div>Lorem ipsum dolor</div>
            <div class="chat-list-item-msg-date">24.07.20 15:40</div>
          </div>
        </div>
        <div class="chat-list-item chat-list-received">
          <div class="chat-list-item-msg">
            <div>Lorem ipsumdolorsitametconsecteturadipisicingelit.</div>
            <div class="chat-list-item-msg-date">24.07.20 15:40</div>
          </div>
        </div>

      </div>
      <div class="chat-msg">
        <div class="chat-msg-input" contenteditable="true" tabindex="0" placeholder="Write a message"></div>
        <div class="chat-msg-options">
          <button class="chat-msg-options-send" title="Send">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
              <path d="M2.01 21L23 12 2.01 3 2 10l15 2-15 2z"/>
            </svg>
          </button>
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
  font-size: .9rem;
}

.widget-chat button {
  font-family: inherit;
  border: none;
  outline: none;
  user-select: none;
  cursor: pointer;
}

.btn-chat-open {
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
.btn-chat-open.new-msg::after {
  content: '';
  position: absolute;
  top: 2px;
  right: 2px;
  background: #FF3D1B;
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

svg.icon-logo {
  fill: none;
  width: 30px;
  height: 30px;
}
svg.icon-logo .fill {
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

.chat {
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
.open .chat {
  opacity: 1;
  transform: scale(1);
}

/************ CHAT HEAD ************/

.chat-head {
  background: #14957B;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  color: #ffffff;
  box-shadow: 0 0 10px rgba(0, 0, 0, .2);
  z-index: 999;
}
.chat-head-close {
  background: none;
  width: 20px;
  height: 20px;
}
.chat-head .icon-close {
  width: 16px;
  height: 16px;
  transition: .1s;
  opacity: .7;
}
.chat-head .icon-close:hover {
  opacity: 1;
}

/************ CHAT LIST ************/

.chat-list {
  overflow-x: hidden;
  overflow-y: auto;
  flex-grow: 1;
}
.chat-list-item {
  padding: 10px;
  display: flex;
}
.chat-list-item.chat-list-sent {
  justify-content: flex-end;
}
.chat-list-item-msg {
  white-space: pre-wrap;
  word-wrap: break-word;
  padding: 10px;
  max-width: 80%;
  color: #555555;
  background: #ffffff;
  border-radius: 10px;
  border-bottom-left-radius: 0;
}
.chat-list-sent .chat-list-item-msg {
  color: #ffffff;
  background: #14957B;
  border-radius: 10px;
  border-bottom-right-radius: 0;
}
.chat-list-item-msg-date {
  display: inline-block;
  background: rgba(0, 0, 0, .1);
  opacity: .7;
  margin-top: 5px;
  padding: 1px 5px;
  border-radius: 5px;
  font-size: .8rem;
}

/************ CHAT MSG ************/

.chat-msg {
  display: flex;
  align-items: center;
  background: #ffffff;
  box-shadow: 0 0 10px rgba(0, 0, 0, .1);
  z-index: 999;
}
.chat-msg-input {
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
.chat-msg-input:empty::before {
  content: attr(placeholder);
  color: #cccccc;
}
.chat-msg-options {
  padding: 10px;
}
.chat-msg-options-send {
  background: none;
  display: flex;
}
.chat-msg-options-send svg {
  fill: #333333;
  width: 20px;
  height: 20px;
}
</style>
