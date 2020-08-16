<template>
  <div>
    <div class="wrapper">
      <div class="wrapper__head">
        <h2 class="wrapper__header">Home page</h2>
      </div>
      <div class="wrapper__content">
        <p>You can open the admin panel and reply to messages <router-link to="/admin" class="wrapper__btn-inline">Admin panel</router-link></p>
        <p>Login: usertest@test.test</p>
        <p>Password: usertest</p>
      </div>
      <div class="wrapper__blocks">
        <div v-for="n in 9" :key="n" class="wrapper__block"></div>
      </div>
    </div>
    <LiveChat/>
  </div>
</template>

<script>
import LiveChat from '@/components/LiveChat'

export default {
  components: {
    LiveChat
  }
}
</script>

<style scoped>
.wrapper {
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
}
.wrapper__head {
  background: #DADADA;
  padding: 20px;
  border-radius: 10px;
  margin-bottom: 20px;
}
.wrapper__header {
  font-size: 1.5rem;
  font-weight: 700;
}
.wrapper__content {
  background: #f2f2f2;
  padding: 10px;
  margin-bottom: 20px;
  border-radius: 10px;
}
.wrapper__content p {
  margin: 10px 0;
}
.wrapper__btn-inline {
  background: #3db29a;
  color: #ffffff;
  padding: 2px 8px;
  border-radius: 6px;
  display: inline-block;
  text-decoration: none;
  transition: .1s;
}
.wrapper__btn-inline:hover {
  background: #24a188;
}
.wrapper__blocks {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 20px;
}
.wrapper__block {
  background: #f2f2f2;
  min-height: 200px;
  border-radius: 10px;
}
@media (max-width: 500px) {
  .wrapper__block {
    min-height: 100px;
  }
}
</style>