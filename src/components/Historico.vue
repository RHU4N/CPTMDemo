<template>
  <div class="historico-container">

    <!-- Conteúdo -->
    <main class="historico-content">

      <!-- Filtros -->
      <div class="filtros">
        <label>
          Status:
          <select v-model="filtroStatus">
            <option value="">Todos</option>
            <option value="enviado">Enviado</option>
            <option value="pendente">Pendente</option>
            <option value="pendenteSync">Pendente Sync</option>
            <option value="sync">Sync</option>
          </select>
        </label>

        <label>
          Linha:
          <input type="text" v-model="filtroLinha" placeholder="Buscar linha..." />
        </label>
      </div>

      <!-- Lista de inspeções filtradas -->
      <InspectionList
        :inspections="inspecoesFiltradas"
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
import { ref, computed } from "vue"
import InspectionList from "../components/InspectionList.vue"

// Controle de filtros
const filtroStatus = ref("")
const filtroLinha = ref("")

// Lista de inspeções (normalmente você pegaria do mesmo state do User)
const inspecoes = ref([
  { id: 1, linha: 'Linha 1', data:'2026-03-05', hora:'10:00', status:'enviado' },
  { id: 2, linha: 'Linha 2', data:'2026-03-05', hora:'10:30', status:'pendente' },
  { id: 3, linha: 'Linha 3', data:'2026-03-05', hora:'11:00', status:'pendenteSync' },
  { id: 4, linha: 'Linha 4', data:'2026-03-05', hora:'11:30', status:'sync' },
])

// Computed para aplicar filtros e ordenar do mais recente para o mais antigo
const inspecoesFiltradas = computed(() => {
  let lista = [...inspecoes.value]

  // Filtro por status
  if(filtroStatus.value) {
    lista = lista.filter(i => i.status === filtroStatus.value)
  }

  // Filtro por linha
  if(filtroLinha.value.trim() !== "") {
    lista = lista.filter(i => i.linha.toLowerCase().includes(filtroLinha.value.toLowerCase()))
  }

  // Ordenar do mais recente para o mais antigo (baseado na data e hora)
  lista.sort((a,b) => {
    const dateA = new Date(`${a.data} ${a.hora}`)
    const dateB = new Date(`${b.data} ${b.hora}`)
    return dateB - dateA
  })

  return lista
})

// Funções de ação (mesmas do User.vue)
function abrirConfigs() { alert("Abrir configurações") }
function editarInspecao(inspecao) { alert(`Editar inspeção: ${inspecao.linha}`) }
function apagarInspecao(inspecao) { 
  const index = inspecoes.value.findIndex(i => i.id === inspecao.id)
  if(index !== -1 && confirm("Tem certeza que deseja apagar esta inspeção?")){
    inspecoes.value.splice(index,1)
  }
}
function enviarInspecao(inspecao){
  inspecao.status = 'sync'
  setTimeout(() => inspecao.status = 'enviado', 2000)
}
function cancelarEnvio(inspecao){ inspecao.status = 'pendente' }
</script>

<style scoped>
.historico-container{
  height:100vh;
  display:flex;
  flex-direction:column;
  background:#f5f5f5;
}

.historico-content{
  flex:1;
  padding:20px;
}

.filtros{
  display:flex;
  flex-wrap:wrap;
  gap:15px;
  margin-bottom:20px;
  justify-content:center;
}

.filtros label{
  display:flex;
  flex-direction:column;
  font-size:14px;
  color:#333;
}

.filtros input, .filtros select{
  margin-top:5px;
  padding:8px 10px;
  border-radius:6px;
  border:1px solid #ccc;
  font-size:14px;
}

@media(min-width:768px){
  .historico-content{
    padding:30px;
  }

  .filtros label{
    font-size:16px;
  }

  .filtros input, .filtros select{
    font-size:16px;
    padding:10px 12px;
  }
}
</style>