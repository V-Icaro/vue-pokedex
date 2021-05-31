<template>
  <div id="app">
    <button class="back" @click="backTop">Topo</button>
    <div class="column is-half is-offset-one-quarter">
      <p class="is-size-1 has-text-weight-bold is-family-code mb-5">Pokedex</p>
      <input
        class="input is-primary"
        type="text"
        placeholder="Buscar pokemon pelo nome"
        v-model="busca"
        @change="buscar"
      />
      <button class="button is-primary mt-2" @click="buscar">Procurar</button>

      <div v-for="(poke, index) in filteredPokemons" :key="poke.name">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";
export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      busca: "",
      filteredPokemons: [],
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        console.log("Dados recebidos");
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
      });
  },
  components: {
    Pokemon,
  },
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons;
      if (this.busca === "" || this.busca == " ") {
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter((pokemon) =>
          pokemon.name.match(this.busca.toLocaleLowerCase())
        );
      }
    },
    backTop: function () {
      window.scrollTo({ top: 0, behavior: "smooth" });
    },
  },
  computed: {
    /*resultadoBusca: function () {
      if (this.busca === "" || this.busca == " ") {
        return this.pokemons;
      } else {
        return this.pokemons.filter((pokemon) => pokemon.name == this.busca);
      }
    },*/
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: #fc4a1a; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to right,
    #f7b733,
    #fc4a1a
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to right,
    #f7b733,
    #fc4a1a
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  height: 100%;
}

.back {
  width: 100px;
  height: 100px;
  display: flex;
  float: right;
  position: fixed;
  margin: 200px;
  background-color: #0a0a0a;
  border-color: transparent;
  color: white;
  border-radius: 50%;
  justify-content: center;
  align-items: center;
}
</style>
