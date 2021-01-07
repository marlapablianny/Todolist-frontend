<template>
  <div id="app">
    <nav>
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="brand-logo center">Tarefas</a>
      </div>
    </nav>
    <div class="container">
      <form v-on:submit.prevent="add">
        <label>Tarefa</label>
        <input type="text" placeholder="Descricao" v-model="descricao_tarefa" />
        <button value="salvar">Adicionar</button>
      </form>
      <div>
        <ul>
          <li
            v-for="(tarefa, index) in tarefas"
            v-bind:key="index"
            v-bind:class="{ marcado: tarefa.checked }"
          >
            {{ tarefa.descricao }}
            <input type="checkbox" v-on:click="check(index)" v-model="tarefa.checked" />
            <button value="salvar" v-on:click="remover(index)">Remover</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template> 
        
<script>
import axios from "axios"
export default {
  name: "App",
  data: function () {
    return {
      descricao_tarefa: "",
      tarefas: [],
    };
  },
  mounted: function(){
    axios.get('localhost:3000/tarefas').then( function (response){
      console.log(response);
    });
  },
  methods: {
    remover: function (index) {
      return this.tarefas.splice(index, 1);
    },
    add: function () {
      this.tarefas.push({
        descricao: this.descricao_tarefa,
        checked: false,
      });
      this.descricao_tarefa = "";
    },
    check: function (index) {
      this.tarefas[index].checked = !this.tarefas[index].checked;
    },
  },
};
</script>

<style>
.marcado {
  background: rgb(31, 109, 83);
}
</style>
