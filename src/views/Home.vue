<template>
  <div>
    <h2>Home page</h2>
    <div>
      <router-link to='/admin'>Admin link</router-link>
    </div>
    <div class="wrapper">Lorem ipsum dolor, sit amet consectetur adipisicing elit. Harum aperiam illum blanditiis reprehenderit sapiente cupiditate explicabo. Inventore aliquam iure neque rem, accusamus iusto tempora obcaecati non sed qui fuga asperiores?</div>
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
  background: #f2f2f2;
  width: 100%;
  max-width: 800px;
  min-height: 200px;
  margin: 0 auto;
  padding: 20px;
}
</style>>