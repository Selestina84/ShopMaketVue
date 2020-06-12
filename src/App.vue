<template>
  <div id="app">
    <div class="container">
      <Header/>
    <main>
      <Products/>
    </main>
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header";
import Products from "@/components/Products";
export default {
  name: "App",
  components: {
    Header,
    Products
  },
  data: {
    
  }
};
</script>

<style lang="sass">
#app
  font-family:Arial, Helvetica, sans-serif
.container
  width: 95%
  margin: 0 auto
  background-color: rgb(204, 201, 201)
</style>
