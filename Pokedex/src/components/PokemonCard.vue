<template>
  <div class="pokemon-card">
    <img :src="pokemon.imageUrl" :class="{ blurred: !pokemon.discovered }" />
    <div v-if="!pokemon.discovered" class="input-container">
      <input v-model="userInput" @keyup.enter="emitDiscover" placeholder="Nombre del Pokémon" />
      <button @click="emitDiscover">Descubrir</button>
    </div>
    <p v-else class="pokemon-name">{{ pokemon.name }}</p>
  </div>
</template>

<script>
export default {
  props: {
    pokemon: Object,
  },
  data() {
    return {
      userInput: '',
    };
  },
  methods: {
    emitDiscover() {
      this.$emit('discover', this.pokemon.id, this.userInput);
      this.userInput = ''; // Limpiar input después de intentar descubrir
    },
  },
};
</script>

<style>
.pokemon-card {
  text-align: center;
}

.blurred {
  filter: blur(5px);
  transition: filter 0.3s;
}

.input-container {
  margin-top: 10px;
}

input {
  margin-bottom: 5px;
  padding: 5px;
}

button {
  padding: 5px 10px;
  background-color: #3b4cca;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #2a379f;
}

.pokemon-name {
  font-weight: bold;
  font-size: 18px;
  color: #333;
}
</style>
