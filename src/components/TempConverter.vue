<template>
  <div class="temperatura">
    <label for="tempInicial">Temperatura Inicial (C°) </label>
    <input type="number" id="tempInicial" v-model="tempInicial" required><br/>

    <label for="tempFinal">Temperatura Final (C°) </label>
    <input type="number" id="tempFinal" v-model="tempFinal" required><br/>
    <input type="button" value="Calcular" @click="gerarLista"/><br/><br/>
    <table>
        <tr>
            <th>Celsius</th>
            <th>Fahrenheit</th>
        </tr>
        <tr v-for='item in listaElementos' :key="item.Celsius">
            <td>
                {{item.Celsius}}
            </td>
            <td>
                {{item.Fahrenheit}}
            </td>
        </tr>

    </table>
  </div>
</template>
<style>
    table, th, td{
        border: 1px solid black;
    }

    .temperatura{
        display: inline-block;
    }
</style>

<script>

export default {
    data(){
        return {
            tempInicial: 0,
            tempFinal: 0,
            listaElementos: [],
    }},

    methods: {
        gerarLista(){
            for (var i = this.tempInicial; i <= this.tempFinal; i++){
                this.listaElementos.push({Celsius: i, Fahrenheit: ((i * 9 / 5) + 32)})
                console.log((this.tempInicial * 9 / 5) + 32)
            }
        }
    }
    
}
</script>

<style>

</style>