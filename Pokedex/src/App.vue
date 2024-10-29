<template>
  <div class="app-container">
    <h1>¿Quién es ese Pokémon?</h1>
    <p>Pokemones descubiertos: {{ discoveredCount }}</p>
    <div class="pokemon-grid">
      <PokemonCard
        v-for="pokemon in pokemons"
        :key="pokemon.id"
        :pokemon="pokemon"
        @discover="checkPokemonName"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';

export default {
  components: { PokemonCard },
  data() {
    return {
      pokemons: [],
      discoveredCount: 0,
    };
  },
  methods: {
    async fetchPokemons() {
      try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20');
        this.pokemons = response.data.results.map((pokemon, index) => ({
          id: index + 1,
          name: pokemon.name,
          imageUrl: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${index + 1}.png`,
          discovered: false,
        }));
      } catch (error) {
        console.error('Error fetching Pokémon:', error);
      }
    },
    checkPokemonName(pokemonId, userInput) {
      const pokemon = this.pokemons.find((p) => p.id === pokemonId);
      if (pokemon && pokemon.name.toLowerCase() === userInput.toLowerCase()) {
        pokemon.discovered = true;
        this.discoveredCount++;
      } else {
        alert('Nombre incorrecto. Intenta de nuevo.');
      }
    },
  },
  computed: {
    discoveredCount() {
      return this.pokemons.filter((pokemon) => pokemon.discovered).length;
    },
  },
  mounted() {
    this.fetchPokemons();
  },
};
</script>

<style>
.app-container {
  text-align: center;
  font-family: Arial, sans-serif;
}

.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  padding: 20px;
}

h1 {
  font-size: 24px;
}

p {
  font-size: 18px;
  color: #333;
}
</style>
