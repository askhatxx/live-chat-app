<template>
  <div class="chat-box__list">
    <div class="chat-box__msg-block chat-box__msg-block_sent">
      <div class="chat-box__msg-text">
        <div>Hello</div>
        <div class="chat-box__msg-date">24.07.20 15:40</div>
      </div>
    </div>
    <div class="chat-box__msg-block chat-box__msg-block_received">
      <div class="chat-box__msg-text">
        <div>Hi!</div>
        <div class="chat-box__msg-date">24.07.20 15:40</div>
      </div>
    </div>
    <div class="chat-box__msg-block chat-box__msg-block_sent">
      <div class="chat-box__msg-text">
        <div>Lorem ipsum dolor, sit amet consectetur adipisicing elit.</div>
        <div class="chat-box__msg-date">24.07.20 15:40</div>
      </div>
    </div>
    <div class="chat-box__msg-block chat-box__msg-block_received">
      <div class="chat-box__msg-text">
        <div>Lorem ipsum dolor, sit amet consectetur adipisicing elit.</div>
        <div class="chat-box__msg-date">24.07.20 16:35</div>
      </div>
    </div>
    <div class="chat-box__msg-block chat-box__msg-block_sent">
      <div class="chat-box__msg-text">
        <div>Lorem ipsum dolor</div>
        <div class="chat-box__msg-date">24.07.20 15:40</div>
      </div>
    </div>
    <div class="chat-box__msg-block chat-box__msg-block_received">
      <div class="chat-box__msg-text">
        <div>Lorem ipsumdolorsitametconsecteturadipisicingelit.</div>
        <div class="chat-box__msg-date">24.07.20 15:40</div>
      </div>
    </div>
  </div>
</template>

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