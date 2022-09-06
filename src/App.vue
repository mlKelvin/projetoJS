<template>
  <div>
    <p>Autor: {{ autor }}</p>
    <p :title="mensagem">{{ new Date().toLocaleString() }}</p>
    <button @click="transformar()">Mudar autor</button>

    <div>
      <h2>Projetos</h2>

      <button @click="mostraFormulario = true" v-show="!mostraFormulario">Formulario</button><br>
      <form @submit.prevent="salvar()" v-show="mostraFormulario">
        <label for="nome">Nome</label>  
        <input type="text" id="nome" v-model="nome" size="30"> <br>
        <label for="duracao">Duracao</label>
        <input type="number" id="duracao" v-model="duracao" required> <br>
        <input type="submit" value="Adicionar">
      </form>

      <input type="text" v-model="filtro" placeholder="Filtro"/>

      <table>
        <tr>
          <th>Id</th>
          <th>Nome</th>
          <th>Duração</th>
        </tr>
        <tr v-for="p in projetosfiltrados" :key="p.id">
          <td>{{ p.id }}</td>
          <td>{{ p.nome }}</td>
          <td>{{ p.duracao }}</td>
          <td><a href="#" @click="excluir(p)">Excluir</a></td>
        </tr>
      </table>
    </div>
  </div>

</template>

<script>
export default{
  data(){
    return{
        //json
        autor: 'kelvin',
        mensagem: 'primeiro app vue',
        maiuscula: true,
        filtro: '',
        mostraFormulario: false,
        projetos: [
          {
            id: 5,
            nome: 'Projeto Java',
            duracao: 10
          },
          {
            id: 6,
            nome: 'Projeto Delphi',
            duracao: 11
          },
          {
            id: 7,
            nome: 'Projeto Python',
            duracao: 12
          }
        ],
        nome: '',
        duracao: null
    }
  },
  
  methods: {
    transformar(){
      if (this.maiuscula) {
        this.autor = this.autor.toLowerCase()
        this.maiuscula = false
      }
      
      else{
        this.autor = this.autor.toUpperCase()
        this.maiuscula = true
      }
    },

    /*getArrayMaxValue(){
      let id = 0;
      this.projetos.forEach(p => {
        if (p.id > id){
          id = this.projetos.id
        }
      })
      return id + 1
    },*/

    salvar() {
      const achou = this.projetos.some(p => p.nome == this.nome)
      if (achou) {
        alert('achou')
      }
      else{
        let maiorId = 0
        this.projetos.forEach(p => maiorId = p.id > maiorId ? p.id : maiorId)
        
        this.projetos.push(
          { 
            id: ++maiorId,
            nome: this.nome,
            duracao: this.duracao
          }
        )
        this.nome = ''
        this.duracao = null
        this.mostraFormulario = false
      } 
    /*  let achou = false
      for(let i = 0; i < this.projetos.size(); i++){
        if (this.projetos[i].nome === this.nome){
          achou = true
        }
      }
      
      
   
      //this.projetos.forEach(p => console.log(p))
      */

    },
    excluir(projeto){
      this.projetos.splice(this.projetos.indexOf(projeto), 1)
    }
  },
  computed: {
    projetosfiltrados(){
      if (this.filtro.trim().length == 0)
      return this.projetos
      return this.projetos.filter(p => p.nome.toLowerCase().includes(this.filtro.toLowerCase())) 
    }
  }
}  
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
