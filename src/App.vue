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
        <input type="text" placeholder="Descricao" v-model="descricao_tarefa"/>
        <button value="salvar">Adicionar</button>
      </form>
      <div>
        <ul>
          <li
            v-for="(tarefa, index) in tarefas"
            v-bind:key="index">
            {{ tarefa }}
            <input type="checkbox" v-model="concluida">
            <button type="submit" value="salvar" v-on:click="remover(index)"></button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template> 
        
<script>
export default {
  name: "App",
  data: function () {
    return {
      tarefas: [],
      id: 0,
      concluida: 0,

    };
  },
  methods: {
    remover: function (index) {
      return this.tarefas.splice(index, 1);
    },
    add: function(){
      this.tarefas.push({
        id: this.id++,
        descricao: this.descricao_tarefa
      })
      this.descricao_tarefa = ''
    },
    //concluida: function (index) {
      //const tarefa = this.tarefas[index];

     // tarefa.concluida = !tarefa.concluida;
      //this.remove(index);
      //if (tarefa.concluida) {
        //this.tarefas.push(tarefa);
      //} else {
       // this.tarefas.splice(0, 0, tarefa);
      //}
    //},
    //remove(index) {
      //this.tarefas.splice(index, 1);
    //},
  },

};
</script>

<style>
</style>
