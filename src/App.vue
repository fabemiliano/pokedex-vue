<template>
  <div id="app">
    <!-- para importar imagens essas devem estar na pasta assests -->
    <div class="header">
      <img src="./assets/pokedex.png" width="200px" />
      <!-- v-model é o two way data binding muito usado em inputs -->
      <input v-model="searched" placeholder="buscar pokemon pelo nome"/>
      <button class="button is-primary" @click="search">Buscar</button>
    </div>
    <div class="column is-half is-offset-one-quarter">
      <!-- para fazer a busca sem botão de busca -->
      <!-- <div v-for="(poke, index) in searchResult" :key="index"> -->
      <!-- o v-for é o map do react, é idêntico quase e também tem que passar a chave única -->
      <div v-for="(poke, index) in filteredPokemons" :key="poke.name">
        <!-- para cada poke é renderizado um componente Pokemon -->
        <!-- a prop se passa assim :nome_da_prop="prop" -->
        <Pokemon :name="poke.name" :url="poke.url" :num="index"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
// para renderizar um componente filho é preciso importá-lo antes
import Pokemon from './components/Pokemon.vue';

export default {
  name: 'App',
  // para rendenrizar componentes é preciso declará-los na propriedade components
  components: {
    Pokemon,
  },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      searched: '',
    };
  },
  created() {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(({ data: { results } }) => { 
      // assim como no react salvo em uma variável o retorno da api
      //aqui no vue pra acessar uma variável sempre uso o this
      this.pokemons = results; 
      this.filteredPokemons = results
      });
  },
  methods: {
    search: function() {
      this.filteredPokemons = this.pokemons
      if (this.search == '' || this.search == ' ') {
        this.filteredPokemons = this.pokemons
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.includes(this.searched))
      }
    },
    console: function(teste) {
      console.log(teste)
    }
  },
  //para fazer a busca sem botão de busca
  // o computed pelo que eu entendi serve para criar propriedades dinâmicas (acho que é similar ao useMemo)
  // computed: {
    // searchResult: function() {
    //   if (this.search == '' || this.search == ' ') {
    //     return this.pokemons
    //   } else {
    //     return this.pokemons.filter(pokemon => pokemon.name.includes(this.search))
    //   }
    // }
  // }
};
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
button {
  background: blue;
}
input {
  width: 50%;
  min-width: 200px;
  height: 40px;
  border-radius: 40px;
  outline: none;
  padding: 5px;
}
.header {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 30vh;
}
</style>
