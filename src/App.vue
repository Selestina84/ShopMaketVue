<template>
  <div id="app" :class="{'overflow-hidden': (this.modalVisibility || this.modalThankVisibility)}">
    <div class="container">
      <Modal :visibility-modal="modalVisibility" @toggle-modal="toggleModal" :API="API"/>
      <Header :basketVisibility="basketVisibility" @toggle-vb="toggleVB" :modalVisibility="modalVisibility" @toggle-modal="toggleModal" @search-text="searchText"/>
      <main>
          <Products :products="filtered"  @add-product="addProduct"/>
          <Basket :basketItems="basketItems" @remove="remove" @make-order="makeOrder" :visibility-basket="basketVisibility"/>
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
    filtered: [],
    basketItems: [],
    basketVisibility: false,
    modalVisibility: false,
    modalThankVisibility: false,
    search: ''
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
    searchText(value){
      let regexp = new RegExp(value, 'i');
      this.filtered = this.products.filter(el => regexp.test(el.title));
    },
    addProduct(item){
      let find = this.basketItems.find(el => el.id === item.id);
      if (find) {
        find.count++;
      } else {
        const prod = Object.assign({ count: 1 }, item);
        this.basketItems.push(prod);
      }
    },
    remove(item){
      if(item.count>1){
          item.count--;
      } else {
          this.basketItems.splice(this.basketItems.indexOf(item), 1);
      }
    },
    createOrder(value){
      return Object.assign({},this.basketItems,{finallyPrice: value})
    },
    makeOrder(allPrice){
      fetch(`${this.API}/basket`, {
            method: "POST",
            body: JSON.stringify(this.createOrder(allPrice)),
            headers: {
                'Content-type': 'application/json'
            }
        })
        .then(response => response.json())
        .then(json =>{
        console.log(json)})
        .catch(error => console.log(error))
        .finally(()=>{
          this.basketItems=[],
          this.basketVisibility=false
        })
    }
  },
  mounted(){
    this._getJson(`${this.API}/products`)
    .then(data => {
      this.products = [...data];
      this.filtered =[... data]
    })
  }
};
</script>

<style lang="sass">
#app
  font-family: Arial, Helvetica, sans-serif
  height: 100vh
body
  padding: 0
  margin: 0
.container
  position: relative
  width: 95%
  height: 100%
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
.overflow-hidden
  overflow: hidden
</style>
