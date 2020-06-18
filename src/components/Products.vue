<template>
  <div class="products">
    <Product  v-for="item of products"
      :key="item.id"
      :product="item"/>
  </div>
</template>

<script>
import Product from "@/components/Product";
export default {
  name: "Products",
  props: ["products"],
  components: {
    Product
  },
  data: () => ({
    
  })
}
</script>

<style lang="sass" scoped>
.products
  padding: 20px
  width: 90%
  height: auto
  margin: 0 auto
  display: flex
  justify-content: space-evenly
  align-items: center
  flex-wrap: wrap
</style>