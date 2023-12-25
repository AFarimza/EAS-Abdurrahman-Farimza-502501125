<script setup>
import TheWelcome from '../components/TheWelcome.vue'
  import { ref } from 'vue'
  const email = ref('')
  const password = ref('')
  const id1 = ref('')
  const email1 = ref('')

  async function login() {
    const res = await fetch('http://lcoalhost:3000/api/users/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        email: email.value,
        password: password.value,
      })
    })

    const json = await res.json()
    console.log(json)

    if(json && json.user) {
      id1.value = json.user.id
      email1.value = json.user.email
    }
  }

  async function getUsers() {
  const res = await fetch('http://lcoalhost:3000/api/users', {
    method: 'GET',
    credentials: 'include',
  })

  const json = await res.json()
  console.log(json)
  users.value = json.docs
}

async function getChats() {
  const res = await fetch('http://lcoalhost:3000/api/chats', {
    method: 'GET',
    credentials: 'include',
  })

  const json = await res.json()
  console.log(json)
  chats.value = json.docs
}
</script>

<template>
  Email: {{ email }}<br>
  Password: {{ password }}<br><br>

  ID1: {{ id1 }}<br>
  Email: {{ email1 }}<br><br>

  ID2: {{ id2 }}<br>
  Email2: {{ email2 }}<br><br>

  <div v-if="id1 == ''">
    <h1>Login</h1><br>
    <input v-model="email" placeholder="email"/><br>
    <input v-model="password" placeholder="password"/><br>
    <button @click="login">Login</button>
  </div>
  <div v-else>
    <h1>Chat</h1><br>
    <li v-for= "user in users">
      <a @click="id2=user.id; email2=user.email">{{ user.email }}</a>
    </li>

    <div v-for="chat in chats">
      <div v-if="chat.from.id == id1 && chat.to.id == id2">
        <div> {{ chat.from.email }} -> {{ chat.to.email }} : {{ chat.message }} </div>
      </div>
      <div v-if="chat.from.id == id2 && chat.to.id == id1"></div>
      <div>{{ chat.from.email }} -> {{ chat.to.email }}</div>
    </div>
  </div>


</template>

