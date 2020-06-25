<template>
  <div class="basket" v-show="visibilityBasket">
    <template v-if="basketItems.length == 0">
      <div class="empty">Your basket is empty</div>
    </template>
    <template v-else>
      <BasketItem v-for="item of basketItems" :key="item.id" :basketItem="item">
      </BasketItem>
      <div class="basket-price">
        Total:
        <span>{{ allCost }} $ </span>
      </div>
      <button class="btn-main basket-btn" @click="$emit('make-order', allCost)">
        Make an order
      </button>
    </template>
  </div>
</template>

<script>
import BasketItem from "@/components/BasketItem";
export default {
  name: "Basket",
  props: ["basketItems", "visibilityBasket"],
  components: {
    BasketItem
  },
  computed: {
    allCost() {
      return this.basketItems.reduce(
        (acc, elem) => (acc += elem.count * elem.price),
        0
      );
    }
  }
};
</script>

<style lang="sass" scoped>
.basket
  width: 20%
  display: block
  padding: 20px
  box-shadow: 0 0 7px rgb(87, 85, 85)
  background-color: rgb(184, 183, 183)
.empty
  color: rgb(87, 85, 85)
  font-weight: bold
  font-size: 20px
  text-align: center
.basket-price
  font-size: 18px
  text-align: end
  margin: 10px 0
  color: rgb(49, 49, 49)
  span
    font-weight: bold
</style>
