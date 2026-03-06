<template>
  <div class="user-container">
    <!-- Header único -->
<AppHeader 
  :title="tituloHeader" 
  @open-configs="abrirConfigs" 
  @click-logo="voltarTelaInicial"
/>

    <!-- Conteúdo principal -->
    <main class="user-content">

      <!-- Tela principal do user -->
      <div v-if="!abrindoFormulario && !abrindoHistorico">
        <p class="intro-text">Escolha uma das funcionalidades abaixo ou veja suas inspeções recentes:</p>

        <div class="button-group">
          <AppButton :icon="inspecaoIcon" @click="abrindoFormulario = true">Inspeção</AppButton>
          <AppButton :icon="historicoIcon" @click="abrindoHistorico = true">Histórico</AppButton>
        </div>

        <InspectionList
          :inspections="inspecoes"
          :show-user="false"
          @edit="editarInspecao"
          @delete="apagarInspecao"
          @send="enviarInspecao"
          @cancel="cancelarEnvio"
        />
      </div>

      <!-- Formulário de inspeção -->
      <InspectionForm
        v-if="abrindoFormulario"
        :inspections="inspecoes"
        @finalizar="finalizarFormulario"
        @fechar="abrindoFormulario = false"
      />

      <!-- Tela de histórico -->
      <Historico
        v-if="abrindoHistorico"
        :inspections="inspecoes"
        @fechar="abrindoHistorico = false"
      />
    </main>
  </div>
</template>

<script setup>
import { reactive, ref, computed } from "vue"
import AppHeader from "../components/AppHeader.vue"
import AppButton from "../components/AppButton.vue"
import InspectionList from "../components/InspectionList.vue"
import InspectionForm from "../components/InspectionForm.vue"
import Historico from "./Historico.vue"

import inspecaoIcon from "../assets/inspecao.png"
import historicoIcon from "../assets/historico.png"

// Flags de exibição
const abrindoFormulario = ref(false)
const abrindoHistorico = ref(false)

function voltarTelaInicial() {
  abrindoFormulario.value = false
  abrindoHistorico.value = false
}
// Lista de inspeções
const inspecoes = reactive([
  { id: 1, linha: 'Linha 1', data:'2026-03-05', hora:'10:00', status:'enviado' },
  { id: 2, linha: 'Linha 2', data:'2026-03-05', hora:'10:30', status:'pendente' },
  { id: 3, linha: 'Linha 3', data:'2026-03-05', hora:'11:00', status:'pendenteSync' },
  { id: 4, linha: 'Linha 4', data:'2026-03-05', hora:'11:30', status:'sync' },
])

// --------------------- Funções ---------------------
function abrirConfigs() { alert("Abrir configurações") }
function editarInspecao(inspecao) { alert(`Editar inspeção: ${inspecao.linha}`) }
function apagarInspecao(inspecao) {
  const index = inspecoes.findIndex(i => i.id === inspecao.id)
  if(index !== -1 && confirm("Tem certeza que deseja apagar esta inspeção?")){
    inspecoes.splice(index,1)
  }
}
function enviarInspecao(inspecao){
  inspecao.status = 'sync'
  setTimeout(() => inspecao.status = 'enviado', 2000)
}
function cancelarEnvio(inspecao){ inspecao.status = 'pendente' }
function finalizarFormulario(dadosFormulario){
  const now = new Date()
  const novaInspecao = {
    id: inspecoes.length + 1,
    linha: `Linha ${inspecoes.length + 1}`,
    data: now.toLocaleDateString(),
    hora: now.toLocaleTimeString(),
    status: 'pendente',
    respostas: dadosFormulario
  }
  inspecoes.unshift(novaInspecao)
  abrindoFormulario.value = false
}

// --------------------- Computed ---------------------
// Título dinâmico do header
const tituloHeader = computed(() => {
  if(abrindoFormulario.value) return "Formulário de Inspeção"
  if(abrindoHistorico.value) return "Histórico de Inspeções"
  return "Bem-vindo, Inspetor"
})
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

.intro-text{
  font-size:1rem;
  color:#333;
  margin-bottom:15px;
  text-align:center;
}

.button-group{
  display:flex;
  flex-wrap:wrap;
  gap:15px;
  margin-bottom:20px;
  justify-content:center;
}

@media(min-width:768px){
  .user-content{
    padding:30px;
  }
  .button-group{
    gap:25px;
  }
  .intro-text{
    font-size:1.2rem;
  }
}
</style>