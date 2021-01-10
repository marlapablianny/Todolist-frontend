<template>
  <div id="app">
    <div class="titulo">
      Todo-List
    </div>
    <div class="container">
      <section class="tarefas">
        <form v-on:submit.prevent="add" class="add">
          <label class="label">Adicione uma Tarefa</label>
          <input type="text" class="texto" placeholder="ex: Lavar louça" v-model="descricao_tarefa">
          <button value="salvar" type="submit" class="button is-primary">Salvar</button>
        </form>
        <ul class="minhas_tarefas">
          <div class="t"
            v-for="(tarefa, index) in tarefas"
            v-bind:key="index"
            v-bind:class="{ marcado: tarefa.concluida }">
            {{ tarefa.descricao }}
            <input
              class="concluida"
              type="checkbox"
              v-on:click="check(index)"
              v-model="tarefa.concluida"
            />
            <button value="salvar" class="remover" v-on:click="remover(index)">Remover</button>
          </div>
        </ul>
      </section>
    </div>
  </div>
</template> 
        
<script>
//Usando axios para consumir a api usando promessas
import axios from "axios";
export default {
  name: "App",
  data: function () {
    return {
      descricao_tarefa: "",
      tarefas: [],
      
    };
  },
  //Fazendo a recuperação dos dados do banco de dados da API, 
  //a função mounted que só deixa todo o codigo rodar após 
  //todos os componentes da API de rederizada;
  mounted: async function () {
    const response = await axios.get("http://localhost:3000/tarefas");
    this.tarefas = response.data;
  },
  //Metodos usadas no projeto
  //todos são funções assicronas, pois uso de promessas, funções que retornam objetos
  // e utiliza promessas para exibir os resultados
  methods: {
    //Remover as tarefas do banco de dados da API
    // Uso de função assicrona, pois a função automaticamente já retorna uma promessa
    //await - espera  a execução da função e resolução da promessa, para retornar o valor, 
    //faz com que a função assicrona não seja finalizada ate que a promessa,seja concluida
    // nesse caso, deletar a tarefa da API seja completada.
    remover: async function (index) {
      const id = this.tarefas[index].id;
      await axios.delete(`http://localhost:3000/tarefas/${id}`);
      this.listar();
    },  
    listar: async function () {
    const response = await axios.get("http://localhost:3000/tarefas");
    this.tarefas = response.data;
  },
    add: async function () {
      const descricao = this.descricao_tarefa;
      await axios.post("http://localhost:3000/tarefas",{descricao});
      this.listar();
      this.descricao_tarefa = "";

    },
    check: async function (index) {
      const id = this.tarefas[index].id;
      await axios.put(`http://localhost:3000/tarefas/${id}`);
      this.listar();
      
    },
  },
};
</script>

<style>
body {
  margin: 0;
}
#app {
  font-family: sans-serif;
}
.marcado {
  text-decoration: line-through;
  color: #ccc;
  text-align: center;

}
.titulo {
  height: 100px;
  background: #00ced1;
  text-align: center;
  font-size: 2rem;
  font-family: sans-serif;
  font-weight: 600;
  line-height: 1.199;
  color: #fff;
  padding-top: 9%;
}
.container {
  padding-top: 200px;
  color: #000;
  display: block;
  margin: 0 3rem;
  box-sizing: inherit;
}
.tarefas {
  height: 450px;
  border: 1px ;
  background-color: #fff;
  box-sizing: inherit;
}
.add {
  max-width: 500px;
  margin: 0 auto;
  background-color: #fff;
  border-radius: 6px;
  box-shadow: 0 0.5em 1em -0.125em rgba(10,10,10,.1), 0 0 0 1px rgba(10,10,10,.02);
  padding: 2.25rem;
}
.label {
  color: #000;
  display: block;
  font-size: 1.2rem;
  font-weight: 700;
  height: 25px;
  
}
.texto{
  height: 30px;
  width: 500px;
  text-align: center;
  border-radius: 80px;
}
.button.is-primary {
  background-color: #00CED1;;
  border-color: transparent;
  color: #fff;
  border-radius: 90px;
  line-height: 10px;
}
.minhas_tarefas {
  margin: 0 auto;
  max-width: 500px;
  line-height: 1.199;
  display: block;
  padding: .3em .75em;
}
.t {
  height: 25px;
  margin: 15px 0 15px 0;
  border-bottom: 1px solid;
  padding: .4em .65em;
}
.concluida {
  float: right;
}
.remover {
  background-color: #00CED1;;
  border-color: transparent;
  color: #fff;
  border-radius: 90px;
  font-family: sans-serif;
  float: right;

}


*, :after, :before {
    box-sizing: inherit;
}

</style>
