<template>
  <div class="pokemon" v-if="props">
    <h1 class="pokemon_name">{{ pokemon.name.toUpperCase() }}</h1>
    <div class="pokemon_types">
      <p
        :style="{
          display: 'flex',
          alignItems: 'center',
          fontSize: '0.75rem',
          gap: '0.5rem',
          padding: '0.25rem 0.5rem',
          borderRadius: '50px',
          background: typeImage[index].color,
          color: 'white',
        }"
        v-for="(type, index) in pokemon.types"
        :key="index"
      >
        <img
          class="pokemon_type_icon"
          :src="require(`../assets/pokeTypeIcons/${type.type.name}.svg`)"
        />
        {{ type.type.name.toUpperCase() }}
      </p>
    </div>
    <img v-bind:src="pokemon.sprites.front_default" />
    <div class="pokemon_characteristics">
      <div class="pokemon_height">
        <span class="pokemon_measurements">HEIGHT</span>
        <span class="pokemon_measurements">{{ pokemon.height / 10 }}m</span>
      </div>
      <div class="pokemon_weight">
        <span class="pokemon_measurements">WEIGHT</span>
        <span class="pokemon_measurements">{{ pokemon.weight / 10 }}kg</span>
      </div>
    </div>
    <div class="pokemon_stats">
      <div
        class="pokemon_stat"
        v-for="(stat, index) in pokemon.stats"
        :key="index"
      >
        <p>{{ statNames[index] }}</p>
        <span>{{ stat.base_stat }}</span>
      </div>
    </div>
  </div>
  <div v-else>Loading</div>
</template>

<script>
import { computed } from "vue";
import { pokeTypes } from "../util/pokeTypes";

export default {
  name: "Pokemon",
  props: ["pokemon"],
  setup(props) {
    const statNames = ["HP", "ATK", "DEF", "SpATK", "SpDEF", "SPD"];
    const typeImage = computed(() => {
      const curPokemonTypes = props.pokemon.types.map((t) => {
        return t.type.name;
      });
      const typeStyles = [];
      for (let i = 0; i < curPokemonTypes.length; i++) {
        for (let j = 0; j < pokeTypes.length; j++) {
          if (pokeTypes[j].type === curPokemonTypes[i]) {
            typeStyles.push(pokeTypes[j]);
          }
        }
      }
      return typeStyles;
    });

    const capitalizeFirst = (s) => {
      return s[0].toUpperCase() + s.slice(1, s.length);
    };

    return {
      props,
      pokeTypes,
      statNames,
      typeImage,
      capitalizeFirst,
    };
  },
};
</script>

<style>
.pokemon {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.pokemon_name {
  font-family: "Game";
  font-size: 1.5rem;
}
.pokemon_types {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}
.pokemon_measurements {
  font-family: "Game";
}

.pokemon_characteristics {
  flex-direction: row;
  display: flex;
  gap: 1rem;
  margin-bottom: 1rem;
}
.pokemon_height,
.pokemon_weight {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}
.pokemon_type_icon {
  height: 0.75rem;
  width: 0.75rem;
}
.pokemon_stats {
  display: flex;
  flex-direction: column;
  width: 25vh;
  border: 1px solid black;
  padding: 0.5rem;
  border-radius: 10px;
}
.pokemon_stat {
  display: flex;
  align-items: center;
  height: 1rem;
  justify-content: space-between;
}

p,
span {
  font-family: "Game";
  font-size: 8px;
  font-weight: bold;
}
</style>
