<template>
  <div>
    <h2>Admin page</h2>
    <div>
      <router-link to="/">Home link</router-link>
    </div>
    <div v-if="isAuth">
      <button @click="signOut">Auth SignOut</button>
      <input v-model="text" placeholder="Text"/>
      <button @click="send">Send text</button>
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
      login: 'user02@test.test',
      password: 'user02',
      text: ''
    }
  },
  mounted() {
    console.log('Admin mounted')
    this.authCheck()
  },
  beforeDestroy() {
    console.log('Admin beforeDestroy')
    if (this.unsubscribeAuth) this.unsubscribeAuth()
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
        });
    },
    authCheck() {
      this.unsubscribeAuth = auth.onAuthStateChanged(user => { // Listener
        console.log('Auth check', user)
        if (user) {
          this.isAuth = true
        } else {
          this.isAuth = false
        }
      })  
    },
    send() {
      db.collection('chat-temp')
        .doc('MwtHjaZIVADLulOwAV2V')
        .get()
        .then(doc => {
          console.log('Get doc --->', doc)
          console.log('Get doc.exists --->', doc.exists)
          console.log('Get doc.data --->', doc.data())

          db.collection('chat-temp')
            .doc('MwtHjaZIVADLulOwAV2V')
            .update({chat: [...doc.data().chat, { text: this.text, date: Date.now(), id: Date.now(), author: 'admin', read: false }]})
            .then(() => {
              console.log('Update doc --->')
            })
            .catch(error => {
              console.log('Auth error', error)
            })

        })
        .catch((error) => {
          console.log('Admin Auth error', error);
        });
    }
  }
}
</script>