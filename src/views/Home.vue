<template>
  <div>
    <h2>Home page</h2>
    <div>
      <router-link to='/admin'>Admin link</router-link>
    </div>
    <LiveChat msg="Welcome!!!"/>
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