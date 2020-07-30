<template>
  <div id="app">
    <LiveChat msg="Welcome"/>
  </div>
</template>

<script>
import LiveChat from './components/LiveChat.vue'

export default {
  name: 'App',
  components: {
    LiveChat
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
