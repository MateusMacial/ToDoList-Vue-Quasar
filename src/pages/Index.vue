<template>
  <div>
    <h2 class="subtitle">Inserir Tarefa</h2>
    
    <q-input outlined v-model.trim="tarefa.text" />

    <q-btn color="primary" label="Add" @click="adicionarTarefa"/>

    <h2 class="subtitle">Lista de Afazeres</h2>
    <ToDoItem
      v-for="(item, index) in lista"

      :tarefa="item.text"
      :realizado="item.realizada"
      :index="index"
      :key="item.text"
      :class="{ tarefaFeita: item.realizada }"
      @feito="feito"
      @deletar="deletar"
    >
    </ToDoItem>
  </div>
</template>

<script>
import ToDoItem from '../components/ToDoItem.vue'
import api from '../services/api.js'

export default {
  components: { ToDoItem },
  name: 'toDoList',
  data () {
    return {
      lista: [],
      tarefa: {
        text: '',
        realizada: false
      }
    }
  },
  methods: {
    adicionarTarefa () {
      let tarefa = {
        text: this.tarefa.text,
        realizada: false
      }
      this.lista.push(tarefa)
      this.tarefa.text = ''
    },
    feito (index) {
      var realizada = this.lista[index].realizada
      if (realizada === true) {
        this.lista[index].realizada = false
      } else {
        this.lista[index].realizada = true
      }
    },
    deletar (index) {
      this.lista.splice(index, 1)
    }
  },
  mounted() {
    api.get('/todo').then(response => {
      console.log(response.data)
    })
  },
}
</script>

<style>
.lista {
  list-style: none;
}
.tarefaFeita p {
  text-decoration: line-through;
}
.subtitle{
  font-family: 'Lucida Sans', 'Lucida Sans Regular';
  padding-left: 20px;
}
</style>
