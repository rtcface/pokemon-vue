<template>
  <div v-if="!pokemon" class="pokemon">
  </div>
  <div v-else>
    <h1>¿Quién es ese pokémon?</h1>  
    <pokemon-picture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>  
    <pokemon-options 
    :pokemons="pokemonArr"
    @selection="checkAnswer"
     />
     <template v-if="showAnswer" >
      <img v-if="showError" src="https://media.tenor.com/jc7s2YjLxfYAAAAC/pokemon-ash.gif" alt="Sos-un-master" class="fade-in">
      <img v-else src="https://gifdb.com/images/thumbnail/pokemon-pikachu-oh-no-ced9qujx28vwv6i6.gif" alt="Sos-un-master" class="fade-in">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="clear()">Nuevo juego</button>
     </template>
     
  </div>
</template>

<script>
import PokemonPicture from '@/components/PokemonPicture.vue'
import PokemonOptions from '@/components/PokemonOptions.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions'

/* console.log(getPokemonOptions()) */

export default {
  components: { PokemonPicture, PokemonOptions },
  data() {
    return {
      pokemonArr:[],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      showError: false,
      
      message:null
    }
  },
  methods:{
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions()
      const rndInt = Math.floor( Math.random() * 4)    
      this.pokemon = this.pokemonArr[ rndInt ]
      this.cls="vi"
    },
    checkAnswer(id) {

      this.showPokemon=true
      this.showAnswer=true
      
      this.pokemon.id === id ? this.message="Correcto Sos Un Maestro Pokemon":this.message=`Ops... era:${this.pokemon.name}`

      this.message === "Correcto Sos Un Maestro Pokemon" ? this.showError=true:this.showError=false

     
    },
    clear(){
      this.pokemonArr=[]
      this.pokemon=null
      this.showError=false
      this.showPokemon=false
      this.showAnswer=false
      this.message=null
      this.mixPokemonArray()

    }
  },
  mounted() {
    this.mixPokemonArray()
  }

}
</script>

<style>
html, body {
  height: 100%;
}

body {
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.pokemon {
  position: relative;
  height: 100px;
  width: 100px;
  background: linear-gradient(to bottom, rgb(254, 0, 1) 50%, white 50% );
  border-radius: 50%;
  border: 8px solid black;
  animation: spin 1s linear infinite;
}

.pokemon::before {
  content: '';
  position: absolute;
  height: 8px;
  width: 100px;
  background: black;
  top: 50px;
  transform: translatey(-50%);
}

.pokemon::after {
  content: '';
  position: absolute;
  height: 38px;
  width: 38px;
  border-radius: 50%;
  background: white;
  top: 50px;
  left: 50px;
  transform: translate(-50%, -50%);
  box-shadow: inset 0 0 0 8px black, inset 0 0 0 10px white, inset 0 0 0 12px black;

}

/* Spin Animation */
@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.vi{
  display: none;
}
</style>
