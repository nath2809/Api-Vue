<template>
  <!-- la variable character debe ser pasada pr un prop, son variables que permiten al componente recibir datos de un padre, en este caso 
    app envia datos al componente character para hacer esto se realiza un script-->
  <div class="column is-12-mobile is-4-desktop is-4-tablet">
    <div class="card">
      <div class="card-header">
        <img v-bind:src="character.image" v-bin:alt="character.name" />
      </div>
      <div class="card-content">
        <h3 class="title is-size-4">{{ character.name }}</h3>
        <button
          class="button is-success is-rounded is-small"
          @click="showMore(character.id)"
        >
          Ver más
        </button>
      </div>
    </div>
  </div>
</template>

<script>
// props que recibe un arreglo con el nombre de las varibles.

export default {
  props: ["character"], // recibe
  methods: {
    showMore(id) {
      this.$emit("showModal", id);
    },
  },
};
</script>
