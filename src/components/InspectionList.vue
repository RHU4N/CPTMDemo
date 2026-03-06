<template>
  <div class="inspection-list">
    <!-- Botão criar inspeção -->
    <button class="create-btn" @click="criarInspecao">+ Nova Inspeção</button>

    <!-- Lista de inspeções -->
    <ul>
      <li v-for="inspecao in localInspections" :key="inspecao.id" :class="inspecao.status">
        <div class="info">
          <span class="linha">{{ inspecao.linha || '—' }}</span>
          <span class="data-hora">{{ inspecao.data }} {{ inspecao.hora }}</span>
          <span v-if="showUser" class="user">{{ inspecao.user }}</span>
          <span class="status">{{ formatStatus(inspecao.status) }}</span>
        </div>

        <div class="actions">
          <!-- Pendente: Editar, Apagar, Enviar -->
          <button v-if="inspecao.status==='pendente'" @click="$emit('edit', inspecao)">Editar</button>
          <button v-if="inspecao.status==='pendente'" @click="$emit('delete', inspecao)">Apagar</button>
          <button v-if="inspecao.status==='pendente'" @click="$emit('send', inspecao)">Enviar</button>

          <!-- PendenteSync: Cancelar e Enviar -->
          <button v-if="inspecao.status==='pendenteSync'" @click="$emit('cancel', inspecao)">Cancelar</button>

          <!-- Sync: Apenas spinner -->
          <span v-if="inspecao.status==='sync'" class="spinner"></span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { reactive, toRefs } from 'vue'

const props = defineProps({
  inspections: { type: Array, required: true },
  showUser: { type: Boolean, default: false }
})

// Cria uma cópia reativa para manipular localmente
const localInspections = reactive([...props.inspections])

function criarInspecao() {
  const id = localInspections.length + 1
  const now = new Date()
  const data = now.toLocaleDateString('pt-BR')
  const hora = now.toLocaleTimeString('pt-BR', { hour: '2-digit', minute: '2-digit' })
  
  let linhaInput = prompt("Digite a linha (opcional, apenas o número):", "")
  let linha = ""

  if(linhaInput) {
    // Se o usuário digitou um número, adiciona "Linha X"
    linha = `Linha ${linhaInput}`
  }

  const nova = { id, linha, data, hora, status: 'pendente' }
  localInspections.push(nova)
}

function apagarInspecao(inspecao) {
  const index = localInspections.findIndex(i => i.id === inspecao.id)
  if(index !== -1) {
    if(confirm("Tem certeza que deseja apagar esta inspeção?")) {
      localInspections.splice(index, 1)
    }
  }
}

// Formata o status
function formatStatus(status) {
  switch(status) {
    case 'enviado': return 'Enviado'
    case 'pendente': return 'Pendente'
    case 'pendenteSync': return 'Pendente (Sync)'
    case 'sync': return 'Sincronizando...'
  }
}
</script>

<style scoped>
.inspection-list {
  margin-top: 20px;
}

.create-btn {
  padding: 10px 15px;
  border: none;
  border-radius: 8px;
  background: #ea191f;
  color: white;
  font-weight: bold;
  cursor: pointer;
  margin-bottom: 15px;
}

.create-btn:hover { opacity:0.9; }

ul {
  list-style:none;
  padding:0;
}

li {
  background:#fff;
  border-radius:8px;
  padding:10px;
  margin-bottom:10px;
  display:flex;
  justify-content:space-between;
  align-items:center;
  flex-wrap:wrap;
  box-shadow: 0 1px 4px rgba(0,0,0,0.1);
}

/* Cores por status */
li.enviado { border-left: 5px solid #28a745; background:#f0fff4; }
li.pendente { border-left: 5px solid #ffc107; background:#fffaf0; }
li.pendenteSync { border-left: 5px solid #17a2b8; background:#f0faff; }
li.sync { border-left: 5px solid #007bff; background:#f0f4ff; }

.info span {
  margin-right:10px;
}

.status {
  font-weight:bold;
}

.actions button {
  margin-left:5px;
  padding:5px 10px;
  border:none;
  border-radius:6px;
  background:#ea191f;
  color:white;
  cursor:pointer;
}

.actions button:hover { opacity:0.9; }

.spinner {
  width:20px;
  height:20px;
  border:3px solid #ccc;
  border-top-color:#007bff;
  border-radius:50%;
  animation:spin 1s linear infinite;
  display:inline-block;
}

@keyframes spin { to { transform: rotate(360deg); } }

@media(min-width:768px){
  li { padding:15px; }
  .actions button { padding:6px 12px; }
  .create-btn { padding:12px 18px; }
}
</style>