<template>
  <div class="basket">
    <template v-if="basketItems.length == 0">
      <div class="empty">Ваша корзина пуста</div>
    </template>
    <template v-else>
      <cart-item
        v-for="item of basketItems"
        :key="item.id"
        :img="img"
        :cart-item="item">
      </cart-item>
      <div class = "basket-price">Итого: {{allCost}} </div>
      <button class="btn basket-btn">Оформить заказ</button>
    </template>
  </div>
</template>

<script>
export default {
  name: "Basket",
  props: ["basketItems"]
}
</script>

<style lang="sass" scoped>

</style>