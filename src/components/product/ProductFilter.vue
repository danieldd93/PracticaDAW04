<template>
  <h6>Filtros</h6>
  <div class="filtro-categoria">
    <label>Categoría:</label>
    <q-select filled v-model="model" :option="categorias" />
  </div>
  <div class="filtro-precio">
    <label>Rango de precios:</label>
    <q-input class="precios" filled v-model="precioMin" type="number" label="Mínimo" />
    <q-input class="precios" filled v-model="precioMax" type="number" label="Máximo" />
  </div>
</template>

<style>
.precios {
  max-width: 200px;
}
</style>
<script>

export default {
  name: "ProductFilter",
  data() {
    return {
      model: null,
      categorias: ["Celulares", "Televisores"]
    }
  }
}
</script>
