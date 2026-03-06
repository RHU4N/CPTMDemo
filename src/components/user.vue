<template>
  <div class="user-container">
    <!-- Header -->
    <AppHeader title="Bem-vindo, Usuário" @open-configs="abrirConfigs" />

    <!-- Conteúdo principal -->
    <main class="user-content">
      <p>Escolha uma das funcionalidades:</p>

      <div class="button-group">
        <!-- Botão Inspeção -->
        <AppButton icon="inspecaoIcon" @click="abrirInspecao">
          Inspeção
        </AppButton>

        <!-- Botão Histórico -->
        <AppButton icon="historicoIcon" @click="abrirHistorico">
          Histórico
        </AppButton>
      </div>
    </main>
  </div>
</template>

<script setup>
import AppHeader from "../components/AppHeader.vue"
import AppButton from "../components/AppButton.vue"
import inspecaoIcon from "../assets/inspecao.png"
import historicoIcon from "../assets/historico.png"

// Funções dos botões
function abrirInspecao() { 
  alert("Abrir tela de Inspeção") 
}

function abrirHistorico() { 
  alert("Abrir tela de Histórico") 
}

function abrirConfigs() { 
  alert("Abrir Configurações") 
}
</script>

<style scoped>
.user-container{
  height:100vh;
  display:flex;
  flex-direction:column;
  background:#f5f5f5;
}

.user-content{
  flex:1;
  padding:20px;
}

.button-group{
  display:flex;
  flex-wrap:wrap;
  gap:15px;
  margin-top:20px;
  justify-content:center;
}

/* Responsivo desktop */
@media (min-width:768px){
  .user-content{
    padding:30px;
  }

  .button-group{
    gap:25px;
  }
}
</style>