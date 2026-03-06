<script setup>
import { reactive, computed } from "vue"

const props = defineProps({
  inspecoes: Array // a inspeção que está sendo preenchida
})

const emit = defineEmits(["finalizar"])

const perguntas = reactive([
  { id: 1, texto: "Linha 1: Verificação do equipamento", tipo:"texto", resposta:"", somenteVisualizacao:false },
  { id: 2, texto: "Linha 2: Checar nível de óleo", tipo:"checkbox", opcoes:["Sim","Não"], resposta:[], somenteVisualizacao:false },
  { id: 3, texto: "Linha 3: Temperatura do motor", tipo:"texto", resposta:"", somenteVisualizacao:false },
  { id: 4, texto: "Linha 4: Sistema de freios", tipo:"checkbox", opcoes:["OK","Não OK"], resposta:[], somenteVisualizacao:true },
  { id: 5, texto: "Linha 5: Luzes e sinalização", tipo:"checkbox", opcoes:["Funcionando","Falha"], resposta:[], somenteVisualizacao:false },
  { id: 6, texto: "Linha 6: Estado da cabine", tipo:"texto", resposta:"", somenteVisualizacao:false },
  { id: 7, texto: "Linha 7: Verificar pneus", tipo:"checkbox", opcoes:["Bom","Ruim"], resposta:[], somenteVisualizacao:false },
  { id: 8, texto: "Linha 8: Limpeza geral", tipo:"texto", resposta:"", somenteVisualizacao:false },
  { id: 9, texto: "Linha 9: Documentação", tipo:"checkbox", opcoes:["Completa","Incompleta"], resposta:[], somenteVisualizacao:true },
  { id:10, texto: "Linha 10: Observações gerais", tipo:"texto", resposta:"", somenteVisualizacao:false },
])

const paginaAtual = reactive({ value: 1 })
const itensPorPagina = 5
const totalPaginas = Math.ceil(perguntas.length / itensPorPagina)

const perguntasPaginaAtual = computed(() => {
  const inicio = (paginaAtual.value - 1) * itensPorPagina
  return perguntas.slice(inicio, inicio + itensPorPagina)
})

function proximaPagina() {
  if (paginaAtual.value < totalPaginas) paginaAtual.value++
}

function paginaAnterior() {
  if (paginaAtual.value > 1) paginaAtual.value--
}

function finalizarInspecao() {
  emit("finalizar", perguntas)
}
</script>

<template>
  <div class="inspection-form">
    <h2>Formulário de Inspeção</h2>
    <div class="form-page">
      <div v-for="pergunta in perguntasPaginaAtual" :key="pergunta.id" class="form-item">
        <label>{{ pergunta.texto }}</label>
        <input v-if="pergunta.tipo==='texto'" type="text" v-model="pergunta.resposta" :readonly="pergunta.somenteVisualizacao"/>
        <div v-else-if="pergunta.tipo==='checkbox'" class="checkbox-group">
          <label v-for="opcao in pergunta.opcoes" :key="opcao">
            <input type="checkbox" v-model="pergunta.resposta" :value="opcao" :disabled="pergunta.somenteVisualizacao"/>
            {{ opcao }}
          </label>
        </div>
      </div>
    </div>
    <div class="navigation">
      <button @click="paginaAnterior" :disabled="paginaAtual.value === 1">Anterior</button>
      <span>Página {{ paginaAtual.value }} de {{ totalPaginas }}</span>
      <button v-if="paginaAtual.value < totalPaginas" @click="proximaPagina">Próxima</button>
      <button v-else @click="finalizarInspecao">Finalizar</button>
    </div>
  </div>
</template>

<style scoped>
.inspection-form {
  max-width: 600px;
  margin: 20px auto;
  background: #fff;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
}

.form-item { margin-bottom: 15px; }
.form-item label { font-weight: bold; display:block; margin-bottom:5px; }
.form-item input[type="text"]{ width:100%; padding:10px; border-radius:6px; border:1px solid #ccc; }
.checkbox-group label{ display:flex; align-items:center; gap:5px; margin-bottom:5px; }

.navigation {
  display:flex;
  justify-content:space-between;
  align-items:center;
  margin-top:20px;
}

.navigation button {
  padding:10px 15px;
  border:none;
  border-radius:6px;
  background:#ea191f;
  color:white;
  font-weight:bold;
  cursor:pointer;
}

.navigation button:disabled {
  opacity:0.5;
  cursor:not-allowed;
}
</style>