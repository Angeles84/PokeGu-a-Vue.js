<template>
  <div class="hello">
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top px-5">
      <a class="navbar-brand" href="#">Guía del Pokémon</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav ml-auto">
          <a class="nav-link active" href="#">Home <span class="sr-only">(current)</span></a>
          <a class="nav-link" href="#">Features</a>
          <a class="nav-link" href="#">Contact</a>
        </div>
      </div>
    </nav>

    <img class="logo img-fluid" src="@/assets/pokemon.png" alt="">
    <h1>{{ titulo }}</h1>

    <label for="">{{ label }}</label>
    <input class="px-2 py-1" type="text" placeholder="Ingrese el nombre" v-model="pk.nombre">
    <button class="btn btn-secondary" type="button" @click="fetchPokemon">Buscar</button>

    <div class="div-card mt-3 text-center">
      <div class="card">
        <center><img class="poke-img img-fluid m-0" :src="imgPokemon.front_default" alt=""></center>
        <h3 class="mb-0">{{pk.name}}</h3>
        <div class="card-body">
          <h5 class="card-title">Movimientos</h5>
          <p class="card-text mb-0" v-for="(movimiento, $index) in movimientos" :key="$index">{{movimiento.move.name}}</p>
          <hr>
          <div>
            <h5 class="card-title">Habilidades</h5>
            <p class="card-text mb-0" v-for="(habilidad, $index) in habilidades" :key="$index">{{habilidad.ability.name}}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokeGuía',
  props: {
    titulo: String,
  },
  data: () => ({
    label: "Nombre del Pokémon:",
    pk: {
      nombre:"",
      movimiento:"",
      movimientos:[],
      habilidad:"",
      habilidades:[],
      sprites: {
        front_default:"",
      },                      
    }
  }),
  computed: {
    imgPokemon() {
      return this.pk.sprites;
    },
    movimientos() {
      return this.pk.moves;
    },
    habilidades() {
      return this.pk.abilities;
    }
  },
  methods: {
    fetchPokemon() {
      fetch(`https://pokeapi.co/api/v2/pokemon/${this.pk.nombre}`)
      .then (response => response.json())
      .then(json => {
        console.log(json)
        this.pk = json;
      })
      .catch(error => {
        alert("Nombre inválido: acaso no te sabes los nombres de los pokémon??")
        console.log(error)
      })  
    }
  },
  created() {
    this.pk.nombre = "pikachu";
    this.fetchPokemon();
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.logo {
  width: 55%;
  margin-top: 2em;
}
h1 {
  font-weight: 700;
  margin-top: 0.7em;
  margin-bottom: 0.5em;
}
h3 {
  font-weight: 700;
  text-decoration: underline;
}
h5 {
  font-weight: 500;
}
input {
  margin-left: 0.5em;
  border: 1.5px solid gray;
  border-radius: 4px;
}
button {
  padding: 0.3em 0.7em;
  margin-left: 0.05em;
  margin-bottom: 0.13em;
}
.div-card {
  width: 50%;
  margin: 0 auto;
  margin-bottom: 3em;
}
.poke-img {
  width: 9em;
}

</style>
