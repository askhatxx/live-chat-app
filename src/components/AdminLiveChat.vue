<template>
  <div class="admin-live-chat">
    <div v-if="isAuth" class="admin-panel">
      <div class="admin-panel__signout">
        <button @click="signOut" class="livechat-btn">Auth SignOut</button>
      </div>
      <AdminChatList 
        @open-chat-id-set="openChatIdSet"
        :chatList="chatList"
        :openChatId="openChatId"
      />
      <AdminOpenChat 
        @send-msg="sendMsg"
        @delete-chat="deleteChat"
        :openChatGet="openChatGet"
      />
    </div>
    <div v-else class="admin-login">
      <input v-model="login" placeholder="Login"/>
      <input v-model="password" placeholder="Password"/>
      <button @click="signIn">Auth SignIn</button>
    </div>
  </div>
</template>

<script>
import AdminChatList from '@/components/AdminChatList'
import AdminOpenChat from '@/components/AdminOpenChat'
import { db, auth } from '@/base'

export default {
  data() {
    return {
      isAuth: false,
      unsubscribeAuth: null,
      unsubscribeDB: null,
      login: 'user02@test.test',
      password: 'user02',
      chatList: [],
      openChatId: null
    }
  },
  components: {
    AdminChatList, AdminOpenChat
  },
  mounted() {
    console.log('AdminLiveChat mounted')
    this.subscribeAuth()
  },
  beforeDestroy() {
    console.log('AdminLiveChat beforeDestroy')
    if (this.unsubscribeAuth) this.unsubscribeAuth()
    if (this.unsubscribeDB) this.unsubscribeDB()
  },
  computed: {
    openChatGet() {
      const activeChat = this.chatList.find(item => item.id === this.openChatId)
      console.log('openChatGet', activeChat)
      if (activeChat) return {...activeChat, success: true}
      else return {success: false}
    }
  },
  methods: {
    async signIn() {
      try {
        await auth.signInWithEmailAndPassword(this.login, this.password)
        console.log('Auth signIn')
      } catch (error) {
        console.log('Auth error', error)
      }
    },
    signOut() {
      auth.signOut()
        .then(() => {
          console.log('Auth signOut')
        })
        .catch((error) => {
          console.log('Auth error', error)
        })
    },
    subscribeAuth() {
      this.unsubscribeAuth = auth.onAuthStateChanged(user => { // Listener
        if (user) {
          this.isAuth = true
          this.subscribeDB()
          console.log('Auth check true')
        } else {
          this.isAuth = false
          console.log('Auth check false')
        }
      })  
    },
    sendMsg(text) {
      console.log('sendMsg text --->', text)
      const activeChat = this.openChatGet
      if (activeChat.success) {
        db.collection('chat-temp')
          .doc(activeChat.id)
          .update({chat: [...activeChat.chat, { text: text, date: Date.now(), id: Date.now(), author: 'admin', read: false }]})
          .then(() => {
            console.log('Update doc --->')
          })
          .catch(error => {
            console.log('Auth error', error)
          })
      }
    },
    deleteChat(id) {
      db.collection('chat-temp')
        .doc(id)
        .delete()
        .then(() => {
          console.log('Delete doc --->', id)
        })
        .catch((error) => {
          console.log('Auth error', error)
        })
    },
    subscribeDB() {
      this.unsubscribeDB = db.collection('chat-temp')
        .onSnapshot(snapshot => {
          const chatList = []
          snapshot.forEach(doc => {
            console.log('Listen collection doc.id ----->', doc.id)
            console.log('Listen collection doc.data', doc.data())
            chatList.push({...doc.data(), id: doc.id})
          })
          this.chatList = chatList.sort((a, b) => {
            const valeuA = a.chat[a.chat.length - 1] && a.chat[a.chat.length - 1].date || 0
            const valeuB = b.chat[b.chat.length - 1] && b.chat[b.chat.length - 1].date || 0
            return valeuB - valeuA
          })
          console.log('chatList', chatList)
        })
    },
    openChatIdSet(id) {
      if (this.openChatId === id) this.openChatId = null
      else this.openChatId = id
      console.log('openChatId', this.openChatId)
    }
  }
}
</script>

<style scoped>
.admin-live-chat {
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
  font-size: 1rem;
}

.admin-panel {
  display: flex;
  flex-wrap: wrap;
}
.admin-panel__signout {
  flex: 0 0 100%;
  padding: 10px;
}
</style>