<template>
  <div>
    <h1>Pokemons</h1>
    <div class="card-container">
      <Pokemon v-for="pokemon of pokemons.results" :name="pokemon.name" :url="pokemon.url" :key="pokemon.url"></Pokemon>
    </div>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Précédent</button>
      <span>{{ currentPage }}</span>
      <button @click="nextPage" :disabled="currentPage * itemsPerPage >= totalPokemons">Suivant</button>
    </div>
  </div>
</template>

<script setup>
  import {ref, onMounted} from 'vue';
  import Pokemon from './components/Pokemon.vue'

  const totalPokemons = ref(0);
  const itemsPerPage = 20; 
  const pokemons = ref([]);
  const currentPage = ref(1);

  async function fetchApi(){
    const offset = (currentPage.value - 1) * itemsPerPage;
    const result = await fetch(`https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=${itemsPerPage}`);
    pokemons.value = await result.json();
    totalPokemons.value = pokemons.value.count;
  }

  const prevPage = () => {
    if (currentPage.value > 1) {
      currentPage.value--;
      fetchApi();
    }
  };

  const nextPage = () => {
    if (currentPage.value * itemsPerPage < totalPokemons.value) {
      currentPage.value++;
      fetchApi();
    }
  };

  onMounted(fetchApi)

</script>

<style scoped>
 .card-container {
      display: flex;
      flex-wrap: wrap; /* Les éléments flexibles vont passer à la ligne si nécessaire */
      justify-content: center; /* Centre les cartes horizontalement */
  }
  .pagination {
    display: flex;
    justify-content: center;
    margin-top: 1rem;
    gap: 10px;
  }
</style>