<template>

  <div class="page">
    <header>
      <h1>Pokemons</h1>
      <input class="searchbar" type="text" v-model="searchQuery" placeholder="Rechercher un Pokémon">
    </header>
    <div class="card-container">
      <Pokemon v-for="pokemon in paginatedPokemons" :name="pokemon.name" :url="pokemon.url" :key="pokemon.url"></Pokemon>
      <h1>{{ paginatedPokemons.count }}</h1>
    </div>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Précédent</button>
      <h3>{{ currentPage }}</h3>
      <button @click="nextPage" :disabled="currentPage >= totalPages">Suivant</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';
import Pokemon from './components/Pokemon.vue'

const itemsPerPage = 20; 
const pokemons = ref([]);
const currentPage = ref(1);
const searchQuery = ref('');

async function fetchAllPokemons(){
  const result = await fetch(`https://pokeapi.co/api/v2/pokemon?limit=1000`);
  const data = await result.json();
  pokemons.value = data.results;
  totalPages.value = Math.ceil(filteredPokemons.value.length / itemsPerPage);
}

onMounted(fetchAllPokemons)

const filteredPokemons = computed(() => {
  let filtered = pokemons.value;
  if (searchQuery.value) {
    filtered = filtered.filter(pokemon => pokemon.name.includes(searchQuery.value.toLowerCase()));
    currentPage.value = 1;
  }
  return filtered;
});

const totalPages = ref(1); // Initialize totalPages

const paginatedPokemons = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage;
  const end = start + itemsPerPage;
  return filteredPokemons.value.slice(start, end);
});

const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--;
  }
};

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
};
</script>

<style scoped>


.page{
  background-color: #14213d;
  min-height: 100vh;
  max-height: 100vh;
}
header{
  display: flex;
  justify-content: space-between;
}
h1{
  color: #fca311;
}
.card-container {
  display: flex;
  flex-wrap: wrap; /* Les éléments flexibles vont passer à la ligne si nécessaire */
  justify-content: center; /* Centre les cartes horizontalement */
}
.searchbar{
  border: 0;
  padding: 0.5rem;
  width: 25%;
}
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 1rem;
  gap: 10px;
  color: #fca311;
}
.pagination button{
  padding: 10px;
}
</style>
