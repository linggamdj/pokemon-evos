<template>
  <div class="row">
    <card v-for="pokemon in pokemons" :key="pokemon.id">
      <template v-slot:title>
        {{ pokemon.name }}
      </template>

      <template v-slot:content>
        <img :src="pokemon.sprite" alt="" />
      </template>

      <template v-slot:description>
        <div v-for="type in pokemon.types" :key="type">
          {{ type }}
        </div>
      </template>
    </card>
  </div>
</template>

<script>
import Card from "./Card.vue";

const api = "https://pokeapi.co/api/v2/pokemon";
const ids = [1, 4, 7];
export default {
  components: {
    Card,
  },

  data() {
    return {
      pokemons: [],
    };
  },

  // Lifecycle hook -> automatically fetch the data as soon as the website visited
  // Use 'created' because no need to access the elements (instead of Mounted)
  created() {
    this.fetchData();
  },

  methods: {
    // async await
    async fetchData() {
      const responses = await Promise.all(
        ids.map((id) => window.fetch(`${api}/${id}`))
      );

      //parsing API to json
      const json = await Promise.all(responses.map((data) => data.json()));

      // getting spesifics data from api
      // note: 'datum' is singular from 'data' (plural)
      this.pokemons = json.map((datum) => ({
        id: datum.id,
        name: datum.name,
        sprite: datum.sprites.other["official-artwork"].front_default,
        types: datum.types.map((type) => type.type.name),
      }));
    },
  },
};
</script>

<style scoped>
.row {
  display: flex;
  justify-content: center;
}

img {
  width: 100%;
}
</style>