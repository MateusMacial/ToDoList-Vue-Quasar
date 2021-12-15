<template>
  <div>
    <h2 class="subtitle">Inserir Tarefa</h2>
    
    <q-input outlined v-model.trim="tarefa.descricao" />

    <q-btn color="primary" label="Add" @click="adicionarTarefa"/>

    <h2 class="subtitle">Lista de Afazeres</h2>
    <ToDoItem
      v-for="(item, index) in lista"
      
      :id="item.id"
      :tarefa="item.descricao"
      :realizada="item.realizada"
      :index="index"
      :key="item.id"
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
        descricao: '',
        realizada: false
      }
    }
  },
  methods: {
    adicionarTarefa () {
      let tarefa = {
        descricao: this.tarefa.descricao,
        realizada: false
      }
      api.post('/todo', tarefa).then(() => {
          api.get('/todo').then(response => {
          this.lista = response.data
          this.tarefa.descricao = ''
        })
      }).catch((error) => {
        var errors = error.response.data.errors
        errors.forEach(x => alert(x.fieldName + ": " + x.message))
      })
    },
    feito (index) {
      var realizada = this.lista[index].realizada
      
      if (realizada === true) {
        this.lista[index].realizada = false
      } 
      else {
        this.lista[index].realizada = true
      }

      var id = this.lista[index].id
      var valorAtualizado = this.lista[index]
      
      api.put('/todo/' + id, valorAtualizado).then(() => {
        api.get('/todo').then(response => {
          this.lista = response.data
        })
      }).catch(() => {
        this.lista[index].realizada = !this.lista[index].realizada
      })
    },
    deletar (index) {
      var id = this.lista[index].id

      api.delete('/todo/' + id).then(() => {
          api.get('/todo').then(response => {
          this.lista = response.data
        })
      })
      
      this.lista.splice(index, 1)
    }
  },
  mounted() {
    api.get('/todo').then(response => {
      this.lista = response.data
    })
  }
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
