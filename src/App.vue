<template>
  <div id="app">
    <div class="page-container">
      <section class="hero"> <!-- Alterado para cor verde -->
        <div class="hero-body">
          <div class="container">
            <h1 class="title">Todolist</h1>
          </div>
        </div>
      </section>

      <section class="section">
        <div class="container">
          <form @submit.prevent="add" class="box">
            <div class="field has-addons">
              <div class="control is-expanded">
                <input
                  type="text"
                  class="input"
                  placeholder="Adicione uma Tarefa"
                  v-model="descricao_tarefa"
                />
              </div>
              <div class="control">
                <button type="submit" class="button is-primary is-outlined">
                  Adicionar
                </button>
              </div>
            </div>
          </form>
          <ul class="minhas_tarefas">
            <li v-for="(tarefa, index) in tarefas" :key="index" class="box">
              <div class="columns is-vcentered">
                <div class="column is-narrow">
                  <label class="checkbox is-primary" :class="{ 'is-completed': tarefa.concluida }">
                    <input type="checkbox" @click="check(index)" v-model="tarefa.concluida"  class="custom-checkbox"/>
                    <span class="checkbox is-primary" :class="{ 'is-visible': tarefa.concluida }"></span>
                  </label>
                </div>
                <div class="column">
                  <span :class="{ 'is-completed': tarefa.concluida }">{{ tarefa.descricao }}</span>
                </div>
                <div class="column is-narrow">
                  <button class="delete is-danger" @click="remover(index)" style="background-color: #ff3860; color: #fff" >Remover</button>
                </div>
              </div>
            </li>
          </ul>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import 'bulma/css/bulma.css';

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
    this.listar();
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
      await axios.post("http://localhost:3000/tarefas", { descricao });
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
  font-family: Arial, sans-serif;
  background-color: #00d1b2; /* Adicione a cor de fundo verde aqui */
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #00d1b2;
}

.page-container {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); /* Sombra suave */
  width: 800px; /* Largura do contêiner */
  max-width: 600px; /* Largura máxima do contêiner */
  padding: 20px;
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
  list-style-type: none;
  padding: 0;
}

.box {
  margin-bottom: 10px;
  border: 1px solid #00d1b2;
}

.checkmark {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 12px;
  height: 6px;
  border-style: solid;
  border-width: 0 2px 2px 0;
  border-color: #fff; /* Cor branca para o risco */
  visibility: hidden;
}

.is-visible {
  visibility: visible;
}

.is-completed {
  text-decoration: line-through;
  color: #888;
}
.title {
  color: #00d1b2;
}


.custom-checkbox {
  content: '';
  appearance: none;
  border: 1px solid #00d1b2;
  border-radius: 4px;
  width: 16px;
  height: 16px;
  outline: none;
}

.custom-checkbox:checked {
  background-image: url("data:image/svg+xml,%0A%3Csvg xmlns='http://www.w3.org/2000/svg' width='17' height='17' viewBox='0 0 10 10'%3E%3Cg class='nc-icon-wrapper' stroke-width='1' fill='%23555555'%3E%3Cpath fill='none' stroke='%23FFFFFF' stroke-linecap='round' stroke-linejoin='round' stroke-miterlimit='10' data-cap='butt' d='M2.83 4.72l1.58 1.58 2.83-2.83'/%3E%3C/g%3E%3C/svg%3E");
  background-color: #00d1b2; /* Cor de fundo quando marcado */
  background-position: center;
  border: none;
  padding: 1px;
}

</style>