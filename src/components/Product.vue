<template>
  <div class="product">
    <h3 class="product-title">{{ product.title }}</h3>
    <img
      class="product-img"
      :alt="product.title"
      src="./../assets/img-default.jpg"
    />
    <p class="product-price">Цена: {{ product.price }} $</p>
    <button class=" btn-main product-btn" @click="addProduct(product)">
      Купить
    </button>
  </div>
</template>

<script>
export default {
  name: "Product",
  props: ["product"],
  methods: {
    addProduct(product) {
      this.$parent.$emit("add-product", product);
    }
  }
};
</script>

<style lang="sass" scoped>
.product
  width: 17%
  margin: 20px
  padding: 20px
  box-shadow: 0 0 7px rgb(87, 85, 85)
  background-color: rgb(184, 183, 183)
  &-title
    font-size: 23px
    font-weight: bold
    text-align: center
    margin: 0 0 20px 0
    color: rgb(87, 85, 85)
  &-img
    display: block
    margin: 0 auto 20px auto
    width: 90%
    height: auto
  &-price
    display: inline-block
    margin: 0 0 18px 0
    color: rgb(87, 85, 85)
    font-size: 20px
    font-weight: 700
  &-btn
    display: block
    width: 80%
    margin: 0 auto
</style>
