<template>
  <div class="about" v-if="pokeData">
    <Pokemon v-bind:pokemon="pokeData" />
  </div>
  <div v-else>Loading..</div>
</template>

<script>
import { ref } from "vue";
import { useRoute } from "vue-router";
import Pokemon from "../components/Pokemon.vue";
export default {
  components: {
    Pokemon,
  },
  setup() {
    const route = useRoute();
    const pokeData = ref();
    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        console.log(data.sprites.other);
        pokeData.value = data;
      });

    return {
      pokeData,
    };
  },
};
</script>
