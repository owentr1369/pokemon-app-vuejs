<template>
  <div class="list">
    <article
      v-for="(pokemon, index) in pokemons"
      :key="'poke' + index"
      @click="setPokemonUrl(pokemon.url)"
    >
      <img
        :src="imageUrl + pokemon.id + '.png'"
        alt=""
        width="96"
        height="96"
      />
      <h3>{{ pokemon.name }}</h3>
    </article>
    <div id="scroll-trigger" ref="infinitescrolltrigger">
      <div class="lds-dual-ring"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["imageUrl", "apiUrl"],

  data: function () {
    return {
      pokemons: [],
      nextUrl: "",
      currentUrl: "",
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.currentUrl);
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
    scrollTrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.next();
          }
        });
      });
      observer.observe(this.$refs.infinitescrolltrigger);
    },
    next() {
      this.currentUrl = this.nextUrl;
      this.fetchData();
    },
    setPokemonUrl(url) {
      this.$emit("setPokemonUrl", url);
    },
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted() {
    this.scrollTrigger();
  },
};
</script>

<style lang="scss">
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
#scroll-trigger {
  display: flex;
  justify-content: center;
  align-items: center;
  .lds-dual-ring {
    display: inline-block;
    width: 80px;
    height: 80px;
  }
  .lds-dual-ring:after {
    content: " ";
    display: block;
    width: 64px;
    height: 64px;
    margin: 8px;
    border-radius: 50%;
    border: 6px solid #fff;
    border-color: #fff transparent #fff transparent;
    animation: lds-dual-ring 1.2s linear infinite;
  }
  @keyframes lds-dual-ring {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
}
</style>