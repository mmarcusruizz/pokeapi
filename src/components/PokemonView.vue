<template>
  <div class="card">
    <!-- Muestra la imagen del Pokémon si 'discovered' es true -->
    <img :src="pokemon.image" :class="{ hidden: !discovered }" alt="Imagen del Pokémon" />
    <!-- Muestra el input y el botón si 'discovered' es false -->
    <div v-if="!discovered">
      <input v-model="userInput" @keyup.enter.prevent="checkName" placeholder="¿Quién es este pokémon?" />
      <button @click="checkName">Show</button>
    </div>
    <!-- Muestra el nombre del Pokémon si 'discovered' es true -->
    <p v-else>{{ pokemon.name }}</p>
  </div>
</template>

<script>
export default {
  // Recibe un objeto 'pokemon' como prop
  props: {
    pokemon: {
      type: Object,
      required: true
    }
  },
  data() {
    // Controla si el Pokémon ha sido descubierto
    return {
      userInput: '',
      discovered: false
    };
  },
  // Método para verificar el nombre ingresado
  methods: {
    checkName() {
      if (this.userInput.trim().toLowerCase() === this.pokemon.name.toLowerCase()) {
        this.discovered = true;
        this.$emit('discovered');
      } else {
        alert('Nombre incorrecto, intenta de nuevo.');
      }
    }
  }
};
</script>

<style scoped>
.card {
  text-align: center;
  padding: 20px;
  border-radius: 25px;
  width: 150px;
  background-color: #e7a305;
  margin: auto;
}

img {
  width: 100px;
  height: 100px;
  transition: filter 0.5s ease;
}

.hidden {
  filter: blur(2px);
}

input {
  border-color: transparent;
  margin-top: 10px;
  border-radius: 10px;
  width: 100%;
}

button {
  margin-top: 10px;
  padding: 10px;
  border-radius: 5px;
  border: none;
  background-color: #00a8b4;
  color: rgb(0, 0, 0);
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #efefef;
}

p {
  color: #efefef;
  margin-top: 10px;
  font-weight: bold;
}
</style>