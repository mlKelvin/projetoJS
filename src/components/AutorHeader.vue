<template>
  <div>
    <p>Autor: {{ autor }}</p>
    <p :title="mensagem">{{ new Date().toLocaleString() }}</p>
    <button @click="transformar()">Mudar autor</button>
  </div>
</template>

<script>
export default {
    props: {
        nome: String,
    },
    data() {
        return {
        autor: null,
        mensagem: "Meu primeiro app vue",
        maiusculaMinuscula: false,
        };
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
        }   
    },
    mounted(){
        this.autor = this.nome
        console.log('mounted')
    },
    watch: {
        nome(value){
            this.autor = value
        }
    }     
}

</script>

<style>

</style>