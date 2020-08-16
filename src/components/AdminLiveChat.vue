<template>
  <div class="admin-live-chat">
    <div class="admin-head">
      <div class="admin-head__header">
        <span>
          <router-link to="/" class="admin-head__btn-header">Home page</router-link>
          <span class="admin-head__title">Admin panel</span>
        </span>
        <button 
          v-if="isAuth"
          @click="signOut"
          class="livechat-btn admin-head__btn-header"
        >
          Logout
        </button>
      </div>
    </div>
    <div v-if="isAuth" class="admin-panel">
      <AdminChatList 
        @open-chat-id-set="openChatIdSet"
        :loadingDB="loadingDB"
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
      <div class="admin-login__box">
        <input 
          v-model="login"
          class="admin-login__input"
          type="text"
          placeholder="Login"
        />
        <input 
          v-model="password"
          @keyup.enter="signIn"
          class="admin-login__input"
          type="password"
          placeholder="Password"
        />
        <div v-if="loginInfo" class="admin-login__info">{{ loginInfo }}</div>
        <button 
          @click="signIn"
          :disabled="loadingAuth"
          class="livechat-btn admin-login__btn-signin"
        >
          Sign in
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import AdminChatList from '@/components/AdminChatList'
import AdminOpenChat from '@/components/AdminOpenChat'
import { dbCollectionChat, auth } from '@/base'

export default {
  data() {
    return {
      isAuth: false,
      unsubscribeAuth: null,
      unsubscribeDB: null,
      loadingAuth: false,
      loadingDB: false,
      login: '',
      password: '',
      loginInfo: '',
      chatList: [],
      openChatId: null
    }
  },
  components: {
    AdminChatList, AdminOpenChat
  },
  mounted() {
    this.subscribeAuth()
  },
  beforeDestroy() {
    if (this.unsubscribeAuth) this.unsubscribeAuth()
    if (this.unsubscribeDB) this.unsubscribeDB()
  },
  computed: {
    openChatGet() {
      const activeChat = this.chatList.find(item => item.id === this.openChatId)
      if (activeChat) return {...activeChat, success: true}
      else return {success: false}
    }
  },
  methods: {
    async signIn() {
      this.loadingAuth = true
      try {
        await auth.signInWithEmailAndPassword(this.login, this.password)
        this.loadingAuth = false
      } catch (error) {
        this.loginInfo = error.message
        this.loadingAuth = false
        console.log('Auth error', error)
      }
    },
    signOut() {
      auth.signOut()
        .then(() => {
          this.loginInfo = ''
        })
        .catch((error) => {
          console.log('Auth error', error)
        })
    },
    subscribeAuth() {
      this.unsubscribeAuth = auth.onAuthStateChanged(user => {
        if (user) {
          this.isAuth = true
          this.subscribeDB()
        } else {
          this.isAuth = false
        }
      })  
    },
    sendMsg(text) {
      const activeChat = this.openChatGet
      if (activeChat.success) {
        dbCollectionChat
          .doc(activeChat.id)
          .update({chat: [...activeChat.chat, { text: text, date: Date.now(), id: Date.now(), author: 'admin', read: false }]})
          .catch(error => {
            console.log('DB error', error)
          })
      }
    },
    deleteChat(id) {
      dbCollectionChat
        .doc(id)
        .delete()
        .catch((error) => {
          console.log('DB error', error)
        })
    },
    subscribeDB() {
      this.loadingDB = true
      this.unsubscribeDB = dbCollectionChat
        .onSnapshot(snapshot => {
          const chatList = []
          snapshot.forEach(doc => {
            chatList.push({...doc.data(), id: doc.id})
          })
          this.chatList = chatList.sort((a, b) => {
            const valeuA = a.chat[a.chat.length - 1] && a.chat[a.chat.length - 1].date || 0
            const valeuB = b.chat[b.chat.length - 1] && b.chat[b.chat.length - 1].date || 0
            return valeuB - valeuA
          })
          if (this.loadingDB) this.loadingDB = false
          this.allMsgRead()
        })
    },
    openChatIdSet(id) {
      if (this.openChatId === id) this.openChatId = null
      else {
        this.openChatId = id
        this.allMsgRead()
      }
    },
    allMsgRead() {
      const activeChat = this.openChatGet
      if (activeChat.success) {
        if (activeChat.chat.some(item => item.author === 'user' && !item.read)) {
          dbCollectionChat
            .doc(activeChat.id)
            .update({chat: activeChat.chat.map(item => {
              if (item.author === 'user') return {...item, read: true}
              return item
            })})
            .catch(error => {
              console.log('DB error', error)
            })
        }

      }
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

.admin-head {
  padding: 10px;
}
.admin-head__header {
  background: #f2f2f2;
  padding: 10px;
  border-radius: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.admin-head__btn-header {
  background: #3db29a;
  color: #ffffff;
  padding: 3px 8px;
  border-radius: 6px;
  display: inline-block;
  font-size: .9rem;
  text-decoration: none;
  transition: .1s;
}
.admin-head__btn-header:hover {
  background: #24a188;
}
.admin-head__title {
  margin-left: 8px;
}

.admin-panel {
  display: flex;
  flex-wrap: wrap;
}

.admin-login {
  padding: 10px;
  display: flex;
  justify-content: center;
}
.admin-login__box {
  background: #14957B;
  padding: 20px;
  border-radius: 10px;
  width: 100%;
  max-width: 400px;
  display: flex;
  flex-direction: column;
}
.admin-login__input {
  background: #3db29a;
  border: none;
  outline: none;
  font-family: inherit;
  border-radius: 6px;
  padding: 8px;
  margin-bottom: 10px;
  color: #ffffff;
  font-size: 1rem;
  transition: .3s;
}
.admin-login__input::placeholder {
  color: #ffffff;
  opacity: .8;
}
.admin-login__input:hover, 
.admin-login__input:focus {
  background: #66c7b3;
}
.admin-login__info {
  background: #f5b1ac;
  border-radius: 6px;
  padding: 8px;
  margin-bottom: 10px;
  color: #751919;
}
.admin-login__btn-signin {
  background: #006c57;
  border-radius: 6px;
  padding: 8px;
  color: #ffffff;
  font-size: 1rem;
  transition: .3s;
}
.admin-login__btn-signin:hover {
  background: #005646;
}
.admin-login__btn-signin:disabled {
  background: #127965;
  color: #afd7cf;
}
</style>