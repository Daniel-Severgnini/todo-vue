<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTmp: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizada: false,
    },
    {
      titulo: 'Estudar Vue.js',
      finalizada: false,
    },
    {
      titulo: 'Ir para a academia',
      finalizada: true,
    }
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const {filtro} = estado;
  
  switch(filtro){
    case 'pendentes':
      return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
        default:
          return estado.tarefas;
  }
}

const cadastraTarefa = () => {
  const novaTarefa = {
    titulo: estado.tarefaTmp,
    finalizada: false,
  }
  estado.tarefas.push(novaTarefa);
  estado.tarefaTmp = '';
}
</script>
<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-tmp="estado.tarefaTmp" :edita-tarefa-tmp="evento => estado.tarefaTmp = evento.target.value" :cadastra-tarefa="cadastraTarefa"/>
    <ListaDeTarefas :tarefas="getTarefasFiltradas()"/>
  </div>
</template>
