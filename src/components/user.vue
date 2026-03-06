<template>
  <div class="user-container">
    <!-- Header -->
    <AppHeader title="Bem-vindo, Usuário" @open-configs="abrirConfigs" />

    <!-- Conteúdo principal -->
    <main class="user-content">
      <!-- Mostrar botões ou formulário -->
      <div v-if="!abrindoFormulario">
        <p class="intro-text">Escolha uma das funcionalidades abaixo ou veja suas inspeções recentes:</p>

        <div class="button-group">
          <AppButton :icon="inspecaoIcon" @click="abrindoFormulario = true">Inspeção</AppButton>
          <AppButton :icon="historicoIcon" @click="abrirHistorico">Histórico</AppButton>
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

      <!-- Formulário de Inspeção -->
      <InspectionForm
        v-else
        :inspections="inspecoes"
        @finalizar="finalizarFormulario"
      />
    </main>
  </div>
</template>

<script setup>
import { reactive, ref } from "vue"
import AppHeader from "../components/AppHeader.vue"
import AppButton from "../components/AppButton.vue"
import InspectionList from "../components/InspectionList.vue"
import InspectionForm from "../components/InspectionForm.vue"

// Ícones dos botões
import inspecaoIcon from "../assets/inspecao.png"
import historicoIcon from "../assets/historico.png"

// Controle de exibição do formulário
const abrindoFormulario = ref(false)

// Lista de inspeções
const inspecoes = reactive([
  { id: 1, linha: 'Linha 1', data:'2026-03-05', hora:'10:00', status:'enviado' },
  { id: 2, linha: 'Linha 2', data:'2026-03-05', hora:'10:30', status:'pendente' },
  { id: 3, linha: 'Linha 3', data:'2026-03-05', hora:'11:00', status:'pendenteSync' },
  { id: 4, linha: 'Linha 4', data:'2026-03-05', hora:'11:30', status:'sync' },
])

// --------------------- Funções ---------------------
// Abrir histórico
function abrirHistorico() { 
  alert("Abrir histórico") 
}

// Abrir configs
function abrirConfigs() { 
  alert("Abrir configurações") 
}

// Editar inspeção
function editarInspecao(inspecao) {
  alert(`Editar inspeção: ${inspecao.linha}`)
}

// Apagar inspeção
function apagarInspecao(inspecao) {
  const index = inspecoes.findIndex(i => i.id === inspecao.id)
  if(index !== -1 && confirm("Tem certeza que deseja apagar esta inspeção?")){
    inspecoes.splice(index,1)
  }
}

// Enviar inspeção para banco
function enviarInspecao(inspecao){
  inspecao.status = 'sync'
  // Simular envio com timeout
  setTimeout(() => {
    inspecao.status = 'enviado'
  }, 2000)
}

// Cancelar envio (pendenteSync)
function cancelarEnvio(inspecao){
  inspecao.status = 'pendente'
}

// Finalizar formulário de inspeção
function finalizarFormulario(dadosFormulario){
  // Criar nova inspeção
  const now = new Date()
  const novaInspecao = {
    id: inspecoes.length + 1,
    linha: `Linha ${inspecoes.length + 1}`,
    data: now.toLocaleDateString(),
    hora: now.toLocaleTimeString(),
    status: 'pendente',
    respostas: dadosFormulario
  }
  inspecoes.unshift(novaInspecao) // adiciona no topo
  abrindoFormulario.value = false
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