<template>
  <div id="app">
    <div class="container">
      <Modal :visibility-modal="modalVisibility" @toggle-modal="toggleModal" :API="API"/>
      <Header :basketVisibility="basketVisibility" @toggle-vb="toggleVB" :modalVisibility="modalVisibility" @toggle-modal="toggleModal"/>
      <main>
          <Products :products="products"  @add-product="addProduct"/>
          <Basket :basketItems="basketItems" @remove="remove" :visibility-basket="basketVisibility"/>
      </main>
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header";
import Products from "@/components/Products";
import Basket from "@/components/Basket";
import Modal from "@/components/Modal";
export default {
  name: "App",
  components: {
    Header,
    Products,
    Basket,
    Modal
  },
  data: () => ({
    API: "http://my-json-server.typicode.com/Selestina84/ShopMaketVue",
    products: [],
    basketItems: [],
    basketVisibility: false,
    modalVisibility: false
  }),
  methods: {
    _getJson(url){
      return fetch(url)
        .then(result => result.json())
        .catch(error => console.log(error));
    },
    toggleVB(){
      this.basketVisibility=!this.basketVisibility
    },
    toggleModal(){
      this.modalVisibility=!this.modalVisibility
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
    },
    remove(item){
      if(item.count>1){
          item.count--;
      } else {
          this.basketItems.splice(this.basketItems.indexOf(item), 1);
      }
    },
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
  position: relative
  width: 95%
  margin: 0 auto
  background-color: rgb(204, 201, 201)
main
  height: 100vh
  padding: 0 5%
  display: flex
  justify-content: space-evenly
  align-items: flex-start
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
