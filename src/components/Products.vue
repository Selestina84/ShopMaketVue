<template>
  <div class="products">
    <Product/>
  </div>
</template>

<script>
import Product from "@/components/Product";
export default {
 name: 'Products',
 componets: {
   Product
 }
}
</script>

<style lang="sass" scoped>
.products
  width: 100%
</style>