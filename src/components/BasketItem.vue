<template>
  <div class="cart-item">
    <div class="basket-product">
      <div class="left-block">
        <p class="basket-product-title">{{ basketItem.title }}</p>
        <img
          class="basket-img"
          src="./../assets/img-default-basket.jpg"
          alt="Some img"
        />
        <p class="product-count">Кол-во: {{ basketItem.count }}</p>
        <p class="product-single-price">Цена {{ basketItem.price }} $</p>
      </div>
      <div class="right-block">
        <button class="delete-btn" @click="$parent.$emit('remove', basketItem)">
          &times;
        </button>
        <p>Общая цена:</p>
        <p class="product-price">{{ basketItem.count * basketItem.price }} $</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "BasketItem",
  props: ["basketItem"]
};
</script>

<style lang="sass" scoped>
.basket-product
  border-bottom: 1px dashed rgb(87, 85, 85)
  padding: 10px
  display: flex
  justify-content: space-between
.basket-product p
  margin: 0 auto 5px auto
  display: inline-block
  color: rgb(87, 85, 85)
  font-weight: bold
.basket-img
  display: block
  margin: 0 auto 5px auto
.left-block,
.right-block
  display: flex
  flex-direction: column
.delete-btn
  margin: 0 0 48px auto
  background-color: red
  width: 30px
  height: 30px
  font-size: 20px
  font-weight: bold
.basket-price
  font-size: 18px
  font-weight: bold
  text-align: end
  margin: 10px 0
  color: rgb(49, 49, 49)
.basket-price p
  display: inline-block
.basket-btn
  width: 70%
  height: 40px
  display: block
  margin: 10px auto
  font-size: 16px
</style>
