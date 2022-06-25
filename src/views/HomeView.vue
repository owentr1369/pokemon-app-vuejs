<template>
  <div class="container">
    <h1>Pokemon's</h1>
    <pokemon-search :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl" />
    <pokemon-list
      :imageUrl="imageUrl"
      :apiUrl="apiUrl"
      @setPokemonUrl="setPokemonUrl"
    />
    <pokemon-details
      v-if="showDetails"
      :pokemonUrl="pokemonUrl"
      :imageUrl="imageUrl"
      @closeDetails="closeDetails"
    />
  </div>
</template>

<script>
import PokemonSearch from "@/components/PokemonSearch.vue";
import PokemonList from "@/components/PokemonList.vue";
import PokemonDetails from "@/components/PokemonDetails.vue";
export default {
  name: "HomeView",
  components: { PokemonSearch, PokemonList, PokemonDetails },
  data: function () {
    return {
      imageUrl:
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/",
      apiUrl: "https://pokeapi.co/api/v2/pokemon?limit=2000",
      pokemonUrl: "",
      showDetails: false,
    };
  },
  methods: {
    setPokemonUrl(url) {
      this.pokemonUrl = url;
      this.showDetails = true;
    },
    closeDetails() {
      this.pokemonUrl = "";
      this.showDetails = false;
    },
  },
};
</script>
