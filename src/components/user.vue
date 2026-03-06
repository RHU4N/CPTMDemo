<template>
  <div class="user-container">
    <!-- Header -->
    <AppHeader title="Bem-vindo, Usuário" @open-configs="abrirConfigs" />

    <main class="user-content">
      <!-- Instruções -->
      <div class="instruction-box">
        <h2>Escolha a funcionalidade</h2>
        <p>
          Você pode realizar inspeções nos locais ou consultar o histórico de atividades já realizadas.
        </p>
      </div>

      <!-- Botões grandes -->
      <div class="button-group">
        <AppButton :icon="inspecaoIcon" @click="abrirInspecao">
          Inspeção
        </AppButton>

        <AppButton :icon="historicoIcon" @click="abrirHistorico">
          Histórico
        </AppButton>
      </div>

      <!-- Lista de inspeções -->
<InspectionList
  :inspections="inspecoes"
  :show-user="false"
  @edit="editarInspecao"
  @delete="apagarInspecao" 
  @send="enviarInspecao"
  @cancel="cancelarEnvio"
/>
    </main>
  </div>
</template>

<script setup>
import { reactive } from "vue"
import AppHeader from "../components/AppHeader.vue"
import AppButton from "../components/AppButton.vue"
import InspectionList from "../components/InspectionList.vue"

import inspecaoIcon from "../assets/inspecao.png"
import historicoIcon from "../assets/historico.png"

// Lista inicial de inspeções
const inspecoes = reactive([
  { id: 1, linha: 'Linha 1', data:'2026-03-05', hora:'10:00', status:'enviado' },
  { id: 2, linha: 'Linha 2', data:'2026-03-05', hora:'10:30', status:'pendente' },
  { id: 3, linha: 'Linha 3', data:'2026-03-05', hora:'11:00', status:'pendenteSync' },
  { id: 4, linha: 'Linha 4', data:'2026-03-05', hora:'11:30', status:'sync' },
])

// Funções dos botões grandes
function abrirInspecao() { alert("Abrir tela de Inspeção") }
function abrirHistorico() { alert("Abrir tela de Histórico") }
function abrirConfigs() { alert("Abrir Configurações") }

// Funções de ações das inspeções
function editarInspecao(inspecao) {
  const novaLinha = prompt("Editar linha:", inspecao.linha)
  if(novaLinha !== null) inspecao.linha = novaLinha
}

function apagarInspecao(inspecao) {
  const index = inspecoes.findIndex(i => i.id === inspecao.id)
  if(index !== -1 && confirm("Tem certeza que deseja apagar esta inspeção?")) {
    inspecoes.splice(index, 1)
  }
}

function enviarInspecao(inspecao) {
  inspecao.status = 'sync'
  setTimeout(() => { inspecao.status = 'enviado' }, 2000)
}

function cancelarEnvio(inspecao) {
  if(inspecao.status === 'pendenteSync') {
    inspecao.status = 'pendente'
  }
}
</script>

<style scoped>
.user-container{
  height:100vh;
  display:flex;
  flex-direction:column;
  background:#f0f2f5;
}

.user-content{
  flex:1;
  padding:20px;
}

/* Caixa de instruções */
.instruction-box {
  background: #ffffff;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
  text-align: center;
  margin-bottom: 25px;
}

.instruction-box h2 {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 10px;
  color: #ea191f;
}

.instruction-box p {
  font-size: 16px;
  color: #333;
  line-height: 1.5;
}

/* Botões grandes */
.button-group{
  display:flex;
  flex-wrap:wrap;
  gap:15px;
  margin-bottom:25px;
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

  .instruction-box h2{
    font-size:24px;
  }

  .instruction-box p{
    font-size:18px;
  }
}
</style>