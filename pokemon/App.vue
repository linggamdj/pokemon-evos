<template>
  <div class="row">
    <div class="card" v-for="p in pokemon" :key="p.id">
      <div class="title">
        {{ p.name }}
      </div>
      <div class="content">
        <img :src="p.sprite" alt="" />
      </div>
      <div class="description">
        <div v-for="type in p.types" :key="type">
          {{ type }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const api = "https://pokeapi.co/api/v2/pokemon";
const ids = [1, 4, 7];
export default {
  data() {
    return {
      pokemon: [],
    };
  },

  // Lifecycle hook -> automatically fetch the data as soon as the website visited
  // Use 'created' because we don't need to access the elements (instead of Mounted)
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
      this.pokemon = json.map((datum) => ({
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
img {
  width: 100%;
}

.row {
  display: flex;
  justify-content: center;
}

.card {
  border: 1px solid silver;
  border-radius: 8px;
  max-width: 200px;
  margin: 0px 5px;
  cursor: pointer;
  box-shadow: 0px 1px 3px darkgrey;
  transition: 0.2s;
}

.title,
.content,
.description {
  padding: 16px;
  text-transform: capitalize;
  text-align: center;
}

.title,
.content {
  border-bottom: 1px solid silver;
}

.title {
  font-size: 1.25em;
}

.card:hover {
  transition: 0.2s;
  box-shadow: 0px 1px 9px darkgrey;
}
</style>