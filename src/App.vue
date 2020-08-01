<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
*, ::after, ::before {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  background: #ffffff;
  font-family: 'Roboto', 'Segoe UI', 'Arial', sans-serif;
  font-size: 1rem;
  color: #000000;
}
</style>
