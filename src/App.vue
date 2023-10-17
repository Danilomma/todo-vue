<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue'
import formulario from './components/formulario.vue'
import Listadetarefas from './components/Listadetarefas.vue'

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizada: false,
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false,
    },
    {
      titulo: 'Ir para a academia',
      finalizada: true,
    }
  ]
})

const getPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const getFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getFiltradas = () => {
  const {filtro} = estado;

  switch (filtro) {
    case 'pendentes':
      return getFiltradas();
      case 'finalizadas':
        return getFinalizadas();
        default:
          return estado.tarefas;
  }
}

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
}

</script>
<template>
  <div class="container">
  <Cabecalho :tarefas-pendentes="getPendentes().length" />
  <formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => evento.target.value" :cadastra-tarefa="cadastraTarefa" />
<Listadetarefas :tarefas="getFiltradas()" />
</div>
</template>


