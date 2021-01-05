<template>
  <div id="pokemon">

	<!-- <div>{{console(pokemon.type)}}</div> -->
		<div class="card">
  <div class="card-image">
    <figure>
			<!-- para passar uma propriedade como src da img precisa fazer um data-biding -->
      <img :src="currentImg" alt="Placeholder image">
    </figure>
  </div>
  <div class="card-content">
    <div class="media">
      <div class="media-content">
				<!-- para usar o filtro primeiro passa a propriedade seguido do pipe e depois do filtro -->
        <p class="title is-4">{{ name | upper }}</p>
				<!-- para pegar o valor de uma propriedade usa-se a notação double moustache -->
        <p class="subtitle is-6">{{	pokemon.type }}</p>
        <!-- <p class="subtitle is-6">{{	console(pokemon.type) }}</p> Aqui fiz um console no template e assim também pode ser usado um método-->
      </div>
    </div>

    <div class="content">
			<!-- @click é um tipo de evento no vue -->
    <button @click="changeSprite()" class="button is-medium is-fullwidth">Mudar Sprite</button>
    </div>
  </div>
</div>
  </div>
</template>

<script>
// os imports vem sempre entre a tag <scrip> e o export default
import axios from 'axios';

export default {
	// pelo que entendi é no data que ficam os estados, seria o this.state. A estrutura é sempre conforme abaixo
	data() {
		return {
			isFront: true,
			currentImg: '',
			pokemon: {
				type: '',
				front: '',
				back: '',
			}
		}
	},
	// pelo que eu entendi os métodos são a forma do vue alterar o estado atual das propriedades, como se fosse o this.setState
	// sendo que o método é chamado pelo evento no template
	methods: {
		changeSprite: function() {
			if (this.isFront) {
				this.isFront = false;
				this.currentImg = this.pokemon.back
			} else {
				this.isFront = true
				this.currentImg = this.pokemon.front
			}
		},
		// o jeito que achei de fazer o console no template foi criando um método 
		console: function(teste) {
			console.log(teste)
		}
	},
	// as props são as propriedades passadas pelo componente pai, o formato parece com a tipagem do TypeScript
  props: {
    num: Number,
    name: String,
    url: String,
	},
	// os filtros são funções que alteram como o dado é visto sem alterar o dado
	filters: {
		upper: function(value) {
			return value[0].toUpperCase() + value.slice(1)
		}
	},
	// created é como se fosse o componentDidMount
	created: function() {
		axios.get(this.url).then(({data}) => {
			this.pokemon.type = data.types[0].type.name; 
			this.pokemon.front = data.sprites.front_default; 
			this.pokemon.back = data.sprites.back_default; 
			this.currentImg = this.pokemon.front;
		});
	}
};
</script>


<style scoped>
	#pokemon {
		margin-top: 10%;
	}
</style>
