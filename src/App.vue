<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div>
          <div class="col-12">
            <h1>{{ title }}</h1>
          </div>
          <div class="d-flex col-12 justify-content-center align-items-center">
            <p class="mx-2">Nombre</p>
            <input
              class="text-center p-1"
              type="text"
              placeholder="del Pokemon"
              v-model="inputPoke"
              @keyup.enter="findPoke"
            />
            <button
              class="btn btn-light mx-2 p-1"
              type="button"
              @click="findPoke"
            >
              Buscar
            </button>
          </div>
          <!--  -->
          <div v-if="pokeData">
            <div class="col-12 my-4">
              <img
                :src="imgPoke"
                :alt="namePoke"
                height="200px"
                v-if="imgPoke"
              />
              <img
                :src="URL_notFound"
                height="300px"
                alt="Imagen no encontrada"
                v-else
              />
            </div>
            <div class="col-12 my-4">
              <div>
                <h2>
                  <strong>{{ titleMove }}</strong>
                </h2>
                <p v-for="(move, i) of movesPoke" :key="i">
                  {{ move }}
                </p>
              </div>
              <div class="my-4">
                <h2>
                  <strong>{{ titleAbility }}</strong>
                </h2>
                <p v-for="(ability, i) of abilityPoke" :key="i">
                  {{ ability }}
                </p>
              </div>
            </div>
          </div>
          <!--  -->
          <div class="m-4" v-else>
            <h2>Pokemon <b>NO</b> encontrado, intenta con otro nombre</h2>
            <img
              :src="URL_notFound"
              height="300px"
              alt="Pokemon no encontrado"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      inputPoke: "pikachu",
      URL_BASE: "https://pokeapi.co/api/v2/pokemon/",
      pokeData: {},
      URL_notFound:
        "https://fotografias-neox.atresmedia.com/clipping/cmsimages02/2016/07/26/CE4896CF-1CCB-4EC7-ACF9-505B8BF2E7C2/98.jpg?crop=640,360,x0,y0&width=1900&height=1069&optimize=high&format=webply",
    };
  },
  created() {
    this.getData();
  },
  computed: {
    title() {
      const title = "PokeGuía";
      return title;
    },
    titleMove() {
      const titleMove = "movimientos";
      return titleMove.toUpperCase();
    },
    titleAbility() {
      const titleAbility = "habilidades";
      return titleAbility.toUpperCase();
    },
    namePoke() {
      return this.pokeData?.name;
    },
    imgPoke() {
      /* return this.pokeData?.sprites?.front_default; */
      return this.pokeData?.sprites?.other?.dream_world?.front_default;
    },
    movesPoke() {
      const moves = this.pokeData?.moves
        .map((move) => move.move?.name)
        .slice(0, 5);
      /* console.log(moves); */
      return moves;
    },
    abilityPoke() {
      const abilities = this.pokeData?.abilities.map(
        (ability) => ability.ability?.name
      );
      return abilities;
    },
  },

  methods: {
    async getData() {
      try {
        const req = await fetch(
          `${this.URL_BASE}${this.inputPoke.trim().toLowerCase()}`
        );
        const data = await req.json();
        this.pokeData = data;
        console.log(this.pokeData);
      } catch (error) {
        console.error(error);
        this.inputPoke = "";
        this.pokeData = false;
      }
    },
    findPoke() {
      this.getData();
    },
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  background-image: url("https://wallpaperaccess.com/full/3551292.jpg");
  background-repeat: no-repeat;
  background-size: cover;
}
.container {
  margin-top: 4rem;
}
h1 {
  color: orange;
}
h2 {
  text-decoration: underline overline;
}
p {
  margin: 0;
  font-weight: 600;
}
</style>
