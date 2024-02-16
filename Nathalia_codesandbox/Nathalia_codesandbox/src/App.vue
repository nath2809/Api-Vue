<template>
  <!-- Contenedor principal de la aplicación -->
  <div id="app">
    <!-- Encabezado de héroe con título y botón de consulta -->
    <div class="hero is-white is-gradient is-bold">
      <div class="hero-body">
        <!-- Título principal -->
        <h1 class="title">
          <span class="has-text-success">R&M</span>
          <span class="subtitle">Personajes</span>
        </h1>
        <div class="field has-addons is-pulled-right">
          <div class="control">
            <input
              v-model="search"
              type="text"
              class="input is-rounded"
              v-on:keyup.enter="searchData"
            />
          </div>

          <div class="control">
            <button
              class="button is-success is-rounded"
              v-on:click="searchData"
            >
              Buscar
            </button>
          </div>
        </div>

        <!-- Botón de consulta -->
      </div>
    </div>
    <div class="container">
      <!-- Iterar sobre la lista de personajes y mostrar tarjetas -->
      <div
        class="columns is-desktop is-mobile is-tablet is-multiline is-centered"
      >
        <character
          @showModal="showModal"
          :character="character"
          v-for="character of characters"
          :key="character.id"
        />
      </div>
      <!-- Permitir el cambio y rango -->
      <nav class="pagination" role="navigation" arial-label="pagination">
        <a class="pagination-previous" v-on:click="changePage(page - 1)"
          >Anterior</a
        >
        <ul class="pagination-list">
          <li>
            <a class="pagination-link is-current">{{ page }}</a>
          </li>
        </ul>
        <a class="pagination-next" v-on:click="changePage(page + 1)"
          >Siguiente</a
        >
      </nav>
    </div>
    <div class="modal" :class="{ 'is-active': modal }" v-if="modal">
      <div class="modal-background" @click="modal = false"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Acerca de: {{ currentCharacter.name }}</p>
        </header>
        <div class="modal-card-body">
          <p>Genero:</p>
          <strong>{{ currentCharacter.gender }}</strong>

          <p>Estatus:</p>
          <strong>{{ currentCharacter.status }}</strong>

          <p>Especie:</p>
          <strong>{{ currentCharacter.species }}</strong>

          <p>Tipo:</p>
          <strong>{{ currentCharacter.type }}</strong>
        </div>

        <footer class="modal-card-foot">
          <button class="button" @click="modal = false">Cerrar</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
//importar librerias
//clase anidada, definir elementos o clases importantes //Clase que permite poner el texto verde
//funcion de vue para realizar el fecth de los datos
import axios from "axios";
import Character from "./components/Character";

export default {
  name: "App",
  // components es un atributo que recibe un objeto
  components: {
    Character, // character
  },

  data: function () {
    // una funcion que retorna un objeto llamado characters
    return {
      characters: [],
      page: 1,
      pages: 1,
      search: "",
      modal: false,
      currentCaracter: {},
    };
  },

  created() {
    //nos indica cuando un componente se a creado en la aplicacion
    // Iniciar el fecth inicial de los datos, desplgar los personajes, cuando la pagina cargue
    this.fetch();
  },

  methods: {
    fetch() {
      const params = {
        page: this.page, //recibe el valor actual de la pagina
        name: this.search,
      };

      let result = axios
        .get("https://rickandmortyapi.com/api/character/", { params }) // recibir parametros en la url
        .then((res) => {
          this.characters = res.data.results;
          console.log(res.data.info); // se le asigna la instancia
          console.log(res.data); //imprimir resultado
        })
        .catch((err) => {
          console.log(err);
        });
    },
    changePage(page) {
      // funcion que recibe la pagina, la pagina que nos envian estan dentro de los rangos
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.fetch();
    },
    searchData() {
      this.page = 1; // buscar y obtener resultados
      this.fetch(); //Reiniciar la busqueda
    },

    showModal(id) {
      this.fetchOne(id);
    },

    async fetchOne(id) {
      let result = await axios.get(
        `https://rickandmortyapi.com/api/character/${id}/`
      );
      this.currentCaracter = result.data;
      this.modal = true; // asisgnar los datos

      console.log(this.currentCaracter, "Personaje");
    },
  },
};
</script>
