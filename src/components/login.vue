<template>
  <div class="login-container">
    
    <img :src="logo" class="logo" alt="Logo">

    <div class="login-box">
      <input 
        v-model="user" 
        type="text" 
        placeholder="Usuário"
      />

      <input 
        v-model="password" 
        type="password" 
        placeholder="Senha"
      />

      <button @click="login">Entrar</button>
    </div>

  </div>
</template>

<script setup>
import { ref } from "vue"
import logo from "../assets/logo.png"

const emit = defineEmits(["login-success"])

const user = ref("")
const password = ref("")

function login(){

  if(user.value === "admin" && password.value === "admin"){
    emit("login-success","admin")
  }

  else if(user.value === "user" && password.value === "user"){
    emit("login-success","user")
  }

  else{
    alert("Usuário ou senha incorretos")
  }

}
</script>

<style>
.login-container{
  height:100vh;
  display:flex;
  flex-direction:column;
  justify-content:center;
  align-items:center;
  background:#f5f5f5;
  padding:20px;
}

.logo{
  width:150px;
  margin-bottom:30px;
}

.login-box{
  display:flex;
  flex-direction:column;
  width:100%;
  max-width:280px;
  gap:10px;
}

.login-box input{
  padding:12px;
  border:1px solid #ccc;
  border-radius:6px;
  font-size:16px;
}

.login-box button{
  padding:12px;
  border:none;
  border-radius:6px;
  background:#ea191f;
  color:white;
  font-weight:bold;
  cursor:pointer;
  font-size:16px;
}

.login-box button:hover{
  opacity:0.9;
}

/* telas maiores (notebook / desktop) */
@media (min-width: 768px){

  .logo{
    width:220px;
  }

  .login-box{
    max-width:400px;
  }

  .login-box input{
    padding:14px;
    font-size:18px;
  }

  .login-box button{
    padding:14px;
    font-size:18px;
  }

}
</style>