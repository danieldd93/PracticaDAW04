<template>
  <div class="product-page">
    <div class="product-filter q-ml-md q-mr-xl">
      <ProductFilter />
    </div>
    <div class="product-list q-mr-md">
      Aquí va el Product List
    </div>
  </div>
</template>
<style>
.product-page {
  display: flex;
  justify-content: space-between;
}

.product-filter {
  width: 25%;
}

.product-list {
  width: 75%;
}
</style>
<script>

import ProductFilter from 'src/components/product/ProductFilter.vue'

export default {
  name: "ProductPage",
  components: {
    ProductFilter
  }
}

</script>
