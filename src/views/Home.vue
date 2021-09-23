<template>
  <div class="home" v-bind:key="pokemon.id" v-for="pokemon in pokemons">
    <router-link class="link" :to="`/about/${urlIdLookup[pokemon.name]}`">
      {{ pokemon.name }}
    </router-link>
  </div>
</template>

<script>
// @ is an alias to /src
import { reactive, toRefs } from "vue";
export default {
  name: "Home",
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
    });

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then((res) => {
        return res.json();
      })
      .then((data) => {
        console.log(data);
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce((acc, cur, idx) => {
          return (acc = { ...acc, [cur.name]: idx + 1 });
        }, {});
      });

    return {
      ...toRefs(state),
    };
  },
};
</script>

<style>
.link {
  text-decoration: none;
  color: black;
  font-family: "Game";
}

.link:hover {
  text-decoration: underline;
}
.link:visited {
  color: inherit;
}
</style>
