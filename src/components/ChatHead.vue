<template>
  <div class="chat-box__head">
    <div class="chat-box__title">LiveChat</div>
    <button class="btn chat-box__btn-close" @click='$emit("open-chat")' title="Close">
      <i class="icon-close icon-close_head"></i>
    </button>
  </div>
</template>

<style scoped>
.btn {
  font-family: inherit;
  border: none;
  outline: none;
  user-select: none;
  cursor: pointer;
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
.icon-close_head {
  width: 16px;
  height: 16px;
  transition: .1s;
  opacity: .7;
}
.icon-close_head:hover {
  opacity: 1;
}

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