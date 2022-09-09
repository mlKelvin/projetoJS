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
        <input type="submit" value="TextoBotaoSalvar">
      </form>

      <input type="text" v-model="filtro" placeholder="Filtro"/> <br>
      <button @click="ordenarPorId()">Ordenar</button>

      <table>
        <tr>
          <th>Id</th>
          <th @click="ordenarPorNome">Nome  <span v-if="ordened" class="material-symbols-outlined">arrow_upward</span>
                                            <span v-else class="material-symbols-outlined">arrow_downward</span> </th>
          <th>Duração</th>
        </tr>
        <tr v-for="p in projetosfiltrados" :key="p.id" @click="selected = p"
                                           :class="{'selected' : isSelected(p)}"
        >
          <td>{{ p.id }}</td>
          <td>{{ p.nome }}</td>
          <td>{{ p.duracao }} -- {{ isSelected(p) }} </td>
          <td><a href="#" @click="excluir(p)">Excluir</a></td>
          <td><a name="atualizar" href="#" @click="update(p)">Editar</a></td>
        </tr>
      </table>
      {{selected}} <br>
      <span style="display: flex; justify-content: left">Duracao total: {{ values() }}</span>
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
        selected: null,
        projetos: [
          {
            id: 9,
            nome: 'o Projeto Java',
            duracao: 10
          },
          {
            id: 6,
            nome: 'Projeto Delphi',
            duracao: 11
          },
          {
            id: 7,
            nome: 'a Projeto Python',
            duracao: 12
          }
        ],
        nome: '',
        duracao: null
    }
  },
  
  methods: {
    limparCampos(){
      this.selected = null
      this.nome = ''
      this.duracao = null
      this.mostraFormulario = false
    },

    update(projeto) {
      this.form_view = true
      this.nome = projeto.nome
      this.duracao = projeto.duracao
    },
    salvar() {
      let sucesso = this.selected == null ? this.inserir() : this.alterar();
      if(sucesso){
        this.limparCampos()
      }
    },

    validar(){
        const achou = this.projetos.some((p) => p != this.selected && p.nome == this.nome);
      if (achou) {
        alert("achou")
        return true
      }
      return false
    },

    inserir() {
      if (! this.validar) {
        this.projetos.push({
          id: this.getMaiorId(),
          nome: this.nome,
          duracao: this.duracao,
        });
        this.nome = "";
        this.duracao = null;
        this.mostraFormulario = false;
      }
    },

    alterar() {
      this.selected.nome = this.nome
      this.selected.duracao = this.duracao
    },

    textoDoBotaoSalvar() {
    return this.selected == null ? "Adicionar" : "Alterar";
    },

    values() {
      let value = 0
      console.log(this.projetos.forEach(p => p.duracao))
      this.projetos.forEach(p => value += p.duracao)
      return value
    },

    ordenarPorNome(){
      if (this.projetos.sort( (a, b) => b.nome.localeCompare(a.nome) ) && this.ordened == false)
      this.ordened = true
      else{
      this.projetos.sort( (a, b) => a.nome.localeCompare(b.nome) )
        this.ordened = false
      }
    },

    ordenarPorId(){
      this.projetos.sort( (a, b) => a.id - b.id)
    },

    isSelected(projeto){
      return this.selected !== null && this.selected.id == projeto.id
    },

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

.selected{
  background-color: pink;
}

.material-symbols-outlined {
  font-variation-settings:
  'FILL' 0,
  'wght' 400,
  'GRAD' 0,
  'opsz' 48
}

/************ COMENTARIOS **************/



    /*getArrayMaxValue(){
      let id = 0;
      this.projetos.forEach(p => {
        if (p.id > id){
          id = this.projetos.id
        }
      })
      return id + 1
    },
    
    inserir() {
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
      let achou = false
      for(let i = 0; i < this.projetos.size(); i++){
        if (this.projetos[i].nome === this.nome){
          achou = true
        }
      }
      //this.projetos.forEach(p => console.log(p))
      
    },*/

</style>