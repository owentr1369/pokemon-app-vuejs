<template>
  <div class="details">
    <div class="details-view" v-if="show">
      <div v-if="pokemon" class="image">
        <img :src="imageUrl + pokemon.id + '.png'" alt="" />
      </div>
      <div v-if="pokemon" class="data">
        <h2>{{ pokemon.name }}</h2>
        <div class="property">
          <div class="left">Base Experience</div>
          <div class="right">{{ pokemon.base_experience }} XP</div>
        </div>
        <div class="property">
          <div class="left">Height</div>
          <div class="right">{{ pokemon.height / 10 }} m</div>
        </div>
        <div class="property">
          <div class="left">Weight</div>
          <div class="right">{{ pokemon.weight / 10 }} kg</div>
        </div>
        <h3>Pokemon types</h3>
        <div class="types">
          <div
            class="type"
            v-for="(value, index) in pokemon.types"
            :key="'value' + index"
          >
            {{ value.type.name }}
          </div>
        </div>
        <h3>Abilities</h3>
        <div class="abilities">
          <div
            class="ability"
            v-for="(value, index) in pokemon.abilities"
            :key="'ability' + index"
          >
            {{ value.ability.name }}
          </div>
        </div>
      </div>
      <h2 v-else>The pokemon was not found</h2>
    </div>
    <div v-else class="lds-dual-ring"></div>
  </div>
</template>

<script>
export default {
  props: ["pokemonUrl", "imageUrl"],
  data: function () {
    return {
      show: false,
      pokemon: {},
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.pokemonUrl);
      console.log("this.pokemonUrl :>> ", this.pokemonUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) {
            return resp.json();
          }
        })
        .then((data) => {
          this.pokemon = data;
          this.show = true;
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
.details {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
  padding: 90px 10px 10px;
  width: calc(100% - 20px);
  height: calc(100vh - 20px);
  background: rgba($color: #000000, $alpha: 0.7);
  .details-view {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    position: relative;
    width: 100%;
    max-width: 510px;
    background-color: #fff;
    padding: 50px 0 0;
    border-radius: 6px;
    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
    .image {
      display: flex;
      justify-content: center;
      align-items: center;
      position: absolute;
      top: -60px;
      width: 120px;
      height: 120px;
      overflow: hidden;
      background: #333;
      border-radius: 50%;
      box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
    }
    h2 {
      text-transform: capitalize;
    }
    .data {
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-direction: column;
      width: 100%;
      margin-bottom: 40px;

      .property {
        width: 90%;
        max-width: 400px;
        border-bottom: 1px solid #ccc;
        margin-bottom: 10px;
        .left {
          float: left;
        }
        .right {
          float: right;
        }
      }
      h3 {
        width: 90%;
        max-width: 400px;
        border-bottom: 1px solid #ccc;
      }
      .types,
      .abilities {
        display: flex;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 90%;
        max-width: 400px;
        .type,
        .ability {
          margin: 0 10px 10px 0;
          padding: 5px 10px;
          border-radius: 20px;
          color: #fff;
          font-size: 1rem;
          letter-spacing: 2px;
          text-transform: capitalize;
          word-wrap: none;
          word-break: keep-all;
        }
        .type {
          background-color: #0a2e50;
        }
        .ability {
          background-color: #c73015;
        }
      }
    }
  }
}
</style>