<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="title-container col-12">
          <h1>{{ title }}</h1>
        </div>
        <div
          class="d-flex col-12 justify-content-center align-items-center nowrap"
        >
          <input
            class="text-center p-1"
            type="text"
            placeholder="Nombre del Pokemon"
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
            <img :src="imgPoke" :alt="namePoke" height="200px" v-if="imgPoke" />
            <img
              :src="URL_notFound"
              height="300px"
              alt="Imagen no encontrada"
              v-else
            />
          </div>
          <div class="description-container col-12 my-4" :class="typeClass">
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
          <img :src="URL_notFound" height="300px" alt="Pokemon no encontrado" />
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
    typePoke() {
      const type = this.pokeData?.types[0].type.name;
      return type;
    },
    typeClass() {
      return {
        typeElectric: this.typePoke == "electric",
        typeFire: this.typePoke == "fire",
        typeWater: this.typePoke == "water",
        typeGrass: this.typePoke == "grass",
        typeBug: this.typePoke == "bug",
        typePoison: this.typePoke == "poison",
        typeGround: this.typePoke == "ground",
        typeFairy: this.typePoke == "fairy",
        typeNormal: this.typePoke == "normal",
        typeFighting: this.typePoke == "fighting",
        typePsychic: this.typePoke == "psychic",
        typeRock: this.typePoke == "rock",
        typeFlying: this.typePoke == "flying",
        typeIce: this.typePoke == "ice",
      };
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
        console.log(this.pokeData.types[0].type.name);
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
  margin-top: 8rem;
}
.title-container {
  max-width: 15rem;
  margin-left: auto;
  margin-right: auto;
}
h1 {
  color: white;
  text-shadow: 2px 2px black;
  box-shadow: 0 0 70px 15px #1adb6ac5;
}
h2 {
  text-decoration: underline overline;
}
p {
  margin: 0;
  font-weight: 600;
}
.description-container {
  max-width: 17rem;
  margin-left: auto;
  margin-right: auto;
  background-color: rgba(5, 5, 5, 0.5);
}

.typeElectric {
  color: #f7d02c;
}
.typeFire {
  color: red;
}
.typeWater {
  color: blue;
}
.typeGrass {
  color: green;
}
.typeBug {
  color: #a6b91a;
}
.typePoison {
  color: purple;
}
.typeGround {
  color: #e2bf65;
}
.typeFairy {
  color: #d685ad;
}
.typeNormal {
  color: #a8a77a;
}
.typeFighting {
  color: #c22e28;
}
.typePsychic {
  color: #f95587;
}

.typeRock {
  color: #b6a136;
}
.typeFlying {
  color: #a98ff3;
}
.typeIce {
  color: #96d9d6;
}
</style>
