<template>
  <div>
    <div class="wrapper">
      <div class="wrapper__head">
        <h2 class="wrapper__header">Home page</h2>
      </div>
      <div class="wrapper__content"><router-link to="/admin">Admin link</router-link> Lorem ipsum dolor, sit amet consectetur adipisicing elit. Harum aperiam illum blanditiis reprehenderit sapiente cupiditate explicabo. Inventore aliquam iure neque rem, accusamus iusto tempora obcaecati non sed qui fuga asperiores?</div>
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
  padding: 10px;
}
.wrapper__head {
  background: #E3E3E3;
  padding: 10px;
  border-radius: 10px;
  margin-bottom: 10px;
}
.wrapper__header {
  font-size: 1.2rem;
  font-weight: 700;
}
.wrapper__content {
  background: #f2f2f2;
  padding: 10px;
  border-radius: 10px;
}
</style>