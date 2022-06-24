<template>
  <div class="list">
    <article v-for="(pokemon, index) in pokemons" :key="'poke' + index">
      <img
        :src="imageUrl + pokemon.id + '.png'"
        alt=""
        width="96"
        height="96"
      />
      <h3>{{ pokemon.name }}</h3>
    </article>
  </div>
</template>

<script>
export default {
  props: ["imageUrl", "apiUrl"],

  data: function () {
    return {
      pokemons: [],
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.apiUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) {
            return resp.json();
          }
        })
        .then((data) => {
          this.nextUrl = data.next;
          data.results.forEach((pokemon, index) => {
            pokemon.id = index + 1;
            this.pokemons.push(pokemon);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style lang="scss" scoped>
.list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  grid-gap: 10px;
  width: 100%;
  max-width: 510px;

  article {
    height: 150px;
    background-color: #efefef;
    text-align: center;
    text-transform: capitalize;
    border-radius: 6px;
    cursor: pointer;

    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
  }
}
</style>