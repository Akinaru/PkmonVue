<template>
      <div class="card">
        <div class="titre">
            <h4 class="nom" v-if="pokemon.name">{{ pokemon.name }}</h4>
            <img :class="{ shinyState: !isShiny }" @click="toggleSprite" class="shiny" src="https://cdn.discordapp.com/attachments/796132389012242473/1217837379558051861/shiny.png?ex=66057a71&is=65f30571&hm=e72cef78e444901d05eb1cbe03a1138350ee4a939ae85f04c26cf559be6481ad&"/>

        </div>
        <div class="image-container">
            <img class="image" v-if="pokemon && pokemon.sprites" :src="urlImg" alt="Sprite">
        </div>
  
      </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';

  const props = defineProps({
      name: String,
      url: String,
  });
  
  const pokemon = ref({});
  const sprites = ref();
  const urlImg = ref('');
  const spriteShiny = ref('');
  const spriteDefault = ref('');
  const isShiny = ref(false);
  
  async function fetchPokemon() {
    const result = await fetch(props.url);
    const pokemonData = await result.json();
    pokemon.value = pokemonData;
    spriteDefault.value = pokemonData.sprites.front_default;
    spriteShiny.value = pokemonData.sprites.front_shiny;
    toggleSprite();
}
function toggleSprite() {
  isShiny.value = !isShiny.value;
  urlImg.value = isShiny.value ? spriteDefault.value : spriteShiny.value;
}
  
  onMounted(fetchPokemon);
  </script>
  
  <style scoped>
  .card-container {
      display: flex;
      flex-wrap: wrap; /* Les éléments flexibles vont passer à la ligne si nécessaire */
      justify-content: center; /* Centre les cartes horizontalement */
  }
  
  .card {
      border: 1px solid black;
      border-radius: 0.5rem;
      max-width: 400px;
      transition: transform 0.3s;
      margin: 0.5rem; 
      overflow: hidden;
  }
  .titre{
    display: flex;
    border-bottom: 1px solid black;
    justify-content: center;
    align-items: center;
    gap: 10%;
  }
  .shiny{
    transition: 0.1s;
    width: 20px;
    height: 30px;
  
  }
  .shiny:hover{
    opacity: 0.6;
    cursor: pointer;
  }
  .shinyState {
    filter: grayscale(100%);
    transition: 0.5s;
  }

  .nom {
      display: flex;
      justify-content: center;
      align-items: center;
      
      line-height: 2.5rem;
      margin: 0;
  }
  .image-container{
    overflow: hidden;
    min-width: 200px;
    max-width: 200px;
    min-height: 200px;
    max-height: 200px;
  }
  .image{
    width: 100%;
    height: 100%;
    transition: transform 0.3s;
    }

    .image:hover {
    transform: scale(1.2);
    }

  </style>
  