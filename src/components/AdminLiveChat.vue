<template>
  <div>
    <div v-if="isAuth">
      <button @click="signOut">Auth SignOut</button>
      <hr/>
      <div>
        <ul>
          <li v-for="chat in chatList" :key="chat.id" @click="openChatIdSet(chat.id)">{{ chat.id }}</li>
        </ul>
      </div>
      <hr/>
      <div>
        <div v-if="openChatGet.success">
          <div v-for="msg in openChatGet.chat" :key="msg.id">{{ msg.text + ' - ' + msg.read}}</div>
          <input v-model="text" placeholder="Text"/>
          <button @click="sendMsg">Send text</button>
        </div>
      </div>
    </div>
    <div v-else>
      <input v-model="login" placeholder="Login"/>
      <input v-model="password" placeholder="Password"/>
      <button @click="signIn">Auth SignIn</button>
    </div>
  </div>
</template>

<script>
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
      openChatId: null,
      text: ''
    }
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
    sendMsg() {
      const activeChat = this.openChatGet
      if (activeChat.success) {
        db.collection('chat-temp')
          .doc(activeChat.id)
          .update({chat: [...activeChat.chat, { text: this.text, date: Date.now(), id: Date.now(), author: 'admin', read: false }]})
          .then(() => {
            console.log('Update doc --->')
          })
          .catch(error => {
            console.log('Auth error', error)
          })
      }
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
      this.openChatId = id
      console.log('openChatId', this.openChatId)
    }
  }
}
</script>