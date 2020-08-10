<template>
  <div>
    <h2>Admin page</h2>
    <div>
      <router-link to="/">Home link</router-link>
    </div>
    <AdminLiveChat/>
  </div>
</template>

<script>
import AdminLiveChat from '@/components/AdminLiveChat'

export default {
  components: {
    AdminLiveChat
  }
}
</script>