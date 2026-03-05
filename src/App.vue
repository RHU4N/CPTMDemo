<script setup>
import { ref, onMounted } from "vue"
import Loading from "./components/loading.vue"
import Login from "./components/login.vue"
import User from "./components/user.vue"
import Admin from "./components/admin.vue"

const loading = ref(true)
const currentScreen = ref("login")

onMounted(() => {
  setTimeout(() => {
    loading.value = false
  }, 3000)
})

function changeScreen(screen){
  currentScreen.value = screen
}
</script>

<template>
  <Loading v-if="loading" />

  <Login
    v-else-if="currentScreen === 'login'"
    @login-success="changeScreen"
  />

  <User v-else-if="currentScreen === 'user'" />

  <Admin v-else-if="currentScreen === 'admin'" />
</template>