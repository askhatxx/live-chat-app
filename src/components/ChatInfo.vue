<template>
  <div :class="'chat-info chat-info_' + info.type">{{ info.text }}</div>
</template>

<script>
export default {
  props: ['info']
}
</script>

<style scoped>
.chat-info {
  background: #DEDEDE;
  color: #333333;
  padding: 10px;
}
.chat-info_error {
  background: #FAD8D8;
  color: #751919;
}
</style>