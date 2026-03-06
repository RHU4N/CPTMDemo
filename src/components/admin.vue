<template>
  <div class="admin-container">
    <!-- Header -->
    <AppHeader 
      :title="tituloHeader" 
      @open-configs="abrirConfigs"
      @click-logo="voltarTelaInicial"
    />

    <main class="admin-content">
      <!-- Tela principal -->
      <div v-if="!abrindoFormulario && !abrindoHistorico">
        <p class="intro-text">Escolha uma das funcionalidades abaixo ou veja as inspeções recentes:</p>

        <!-- Botões -->
        <div class="button-group">
          <AppButton :icon="inspecaoIcon" @click="abrindoFormulario = true">Inspeção</AppButton>
          <AppButton :icon="historicoIcon" @click="abrindoHistorico = true">Histórico</AppButton>
          <AppButton :icon="adminIcon" @click="abrindoAdmin = true">Admin</AppButton>
          <AppButton :icon="logsIcon" @click="abrindoLogs = true">Logs</AppButton>
          <AppButton :icon="chamadosIcon" @click="abrindoChamados = true">Chamados</AppButton>
        </div>

        <!-- Mapa + Lista de usuários -->
        <div class="mapa-lista">
          <div class="mapa">
            <Mapa :usuarios="usuarios" :admin-loc="adminLoc" />
          </div>
          <div class="lista-usuarios">
            <h3>Usuários</h3>
            <ul>
              <li v-for="u in usuarios" :key="u.id">
                {{ u.nome }} - <span :class="u.status">{{ u.status }}</span>
              </li>
            </ul>
          </div>
        </div>

        <!-- Lista de inspeções -->
        <InspectionList
          :inspections="inspecoes"
          :show-user="true"
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

      <!-- Outras telas -->
      <div v-if="abrindoAdmin"><p>Tela de administração</p></div>
      <div v-if="abrindoLogs"><p>Tela de logs</p></div>
      <div v-if="abrindoChamados"><p>Tela de chamados</p></div>
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
import Mapa from "../components/mapa.vue"

// Ícones
import inspecaoIcon from "../assets/inspecao.png"
import historicoIcon from "../assets/historico.png"
import adminIcon from "../assets/admin.jpg"
import logsIcon from "../assets/logs.png"
import chamadosIcon from "../assets/chamados.png"

// Flags
const abrindoFormulario = ref(false)
const abrindoHistorico = ref(false)
const abrindoAdmin = ref(false)
const abrindoLogs = ref(false)
const abrindoChamados = ref(false)

// Lista de inspeções
const inspecoes = reactive([
  { id: 1, linha: 'Linha 1', data:'2026-03-05', hora:'10:00', status:'enviado', usuario:'user1' },
  { id: 2, linha: 'Linha 2', data:'2026-03-05', hora:'10:30', status:'pendente', usuario:'user2' },
  { id: 3, linha: 'Linha 3', data:'2026-03-05', hora:'11:00', status:'pendenteSync', usuario:'user1' },
  { id: 4, linha: 'Linha 4', data:'2026-03-05', hora:'11:30', status:'sync', usuario:'user3' },
])

// Lista de usuários com coordenadas
const usuarios = reactive([
  { id:1, nome:'user1', status:'online', coords:[-23.55052, -46.633308] },
  { id:2, nome:'user2', status:'offline', coords:[-23.56052, -46.643308] },
  { id:3, nome:'user3', status:'online', coords:[-23.54052, -46.623308] },
])

// Admin loc (vai pegar geolocalização)
const adminLoc = ref(null)
if(navigator.geolocation){
  navigator.geolocation.getCurrentPosition(pos=>{
    adminLoc.value = [pos.coords.latitude, pos.coords.longitude]
  }, ()=> {
    adminLoc.value = [-23.55052, -46.633308] // fallback
  })
}

// ---------------- Funções ----------------
function abrirConfigs() { alert("Abrir configurações") }
function editarInspecao(inspecao) { alert(`Editar inspeção: ${inspecao.linha}`) }
function apagarInspecao(inspecao){
  const index = inspecoes.findIndex(i => i.id === inspecao.id)
  if(index!==-1 && confirm("Tem certeza que deseja apagar esta inspeção?")){
    inspecoes.splice(index,1)
  }
}
function enviarInspecao(inspecao){
  inspecao.status = 'sync'
  setTimeout(()=> inspecao.status='enviado',2000)
}
function cancelarEnvio(inspecao){ inspecao.status='pendente' }
function finalizarFormulario(dadosFormulario){
  const now = new Date()
  const novaInspecao = {
    id: inspecoes.length + 1,
    linha:`Linha ${inspecoes.length + 1}`,
    data: now.toLocaleDateString(),
    hora: now.toLocaleTimeString(),
    status:'pendente',
    usuario:'admin',
    respostas: dadosFormulario
  }
  inspecoes.unshift(novaInspecao)
  abrindoFormulario.value = false
}

// Computed
const tituloHeader = computed(()=>{
  if(abrindoFormulario.value) return "Formulário de Inspeção"
  if(abrindoHistorico.value) return "Histórico de Inspeções"
  if(abrindoAdmin.value) return "Administração"
  if(abrindoLogs.value) return "Logs"
  if(abrindoChamados.value) return "Chamados"
  return "Bem-vindo, Admin"
})

// Voltar à tela principal
function voltarTelaInicial(){
  abrindoFormulario.value=false
  abrindoHistorico.value=false
  abrindoAdmin.value=false
  abrindoLogs.value=false
  abrindoChamados.value=false
}
</script>

<style scoped>
.admin-container{ height:100vh; display:flex; flex-direction:column; background:#f5f5f5; }
.admin-content{ flex:1; padding:20px; }
.intro-text{ font-size:1rem; color:#333; margin-bottom:15px; text-align:center; }
.button-group{ display:flex; flex-wrap:wrap; gap:15px; margin-bottom:20px; justify-content:center; }

/* Mapa + Lista */
.mapa-lista{ display:flex; flex-direction:column; gap:15px; margin-bottom:20px; height:300px; }
.mapa{ flex:0 0 60%; height:100%; }
.lista-usuarios{ flex:0 0 40%; background:#eee; padding:10px; overflow-y:auto; }
.lista-usuarios li{ list-style:none; margin-bottom:5px; }
.online{ color:green; }
.offline{ color:red; }

@media(min-width:768px){
  .admin-content{ padding:30px; }
  .button-group{ gap:25px; }
  .intro-text{ font-size:1.2rem; }
  .mapa-lista{ flex-direction:row; height:300px; }
}
</style>