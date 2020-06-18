<template>
  <div id="app">
    <div class="container">
      <Header/>
    <main>
        <Products :products="products" @add-product="addProduct"/>
        <Basket :basketItems="basketItems"/>
    </main>
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header";
import Products from "@/components/Products";
import Basket from "@/components/Basket";
export default {
  name: "App",
  components: {
    Header,
    Products,
    Basket
  },
  data: () => ({
    API: "http://my-json-server.typicode.com/Selestina84/ShopMaketVue",
    products: [],
    basketItems: []
  }),
  methods: {
    _getJson(url){
      return fetch(url)
        .then(result => result.json())
        .catch(error => console.log(error));
    },
    addProduct(item){
      let find = this.basketItems.find(el => el.id === item.id);
      if (find) {
        find.count++;
      } else {
        const prod = Object.assign({ count: 1 }, item);
        this.basketItems.push(prod);
      }
      console.log(this.basketItems);
    }
  },
  mounted(){
    this._getJson(`${this.API}/products`)
    .then(data => {
      this.products = [...data];
    })
  }
};
</script>

<style lang="sass">
#app
  font-family: Arial, Helvetica, sans-serif
.container
  width: 95%
  margin: 0 auto
  background-color: rgb(204, 201, 201)
.btn-main
  height: 50px
  background: linear-gradient(to bottom, rgb(110, 109, 109), rgb(238, 235, 235) 50%, rgb(105, 105, 105))
  font-weight: bold
  color: rgb(87, 85, 85)
  font-size: 18px
  &:hover
    cursor: pointer
    transform: scale(1.05)
  &:focus
    outline: none
</style>
