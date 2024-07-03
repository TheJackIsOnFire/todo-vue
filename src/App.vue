<script setup>
  import { reactive } from 'vue'; //Importando o eatdo reativo
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

//criando um estado reativo
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

  //Captura as tarefas que estão pendentes ou seja, com valor false
  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada);
  }

  //Captura as tarefas que estão finalizadas ou seja, com valor true
  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada);
  }

  //Usa a desestruturação para acessar a propriedade filtro do estado
  const  getTarefasFiltradas = () => {
    const {filtro} = estado;

    switch(filtro) { //O switch testa varias condicionais e envia retornos 
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return  getTarefasFinalizadas();
      default:
        return estado.tarefas;  
    }
  }

  //Captura o valor da tarefa digitada no formulario e adiciona na propriedade tarefas no estado
  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova)
    estado.tarefaTemp = '';
  }
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa"/>
    <ListaDeTarefas :tarefas="getTarefasFiltradas()"/>   
  </div>  
</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
