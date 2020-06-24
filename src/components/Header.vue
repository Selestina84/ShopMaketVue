<template>
  <header class="header">
    <div class="left-wrapper">
      <a href="#" class="header-link">
        <img src="./../assets/logo.png" alt="logo" class="header-logo" />
      </a>
    </div>
    <div class="rigth-wrapper">
      <button class="btn-main btn-modal" @click="toggleModalVisibility">
        Связаться с нами
      </button>
      <Search />
      <button class="btn-main btn-basket" @click="toggleBasketVisibility">
        Корзина
      </button>
    </div>
  </header>
</template>

<script>
import Search from "@/components/Search";
export default {
  name: "Header",
  props: ["basketVisibility", "modalVisibility"],
  components: {
    Search
  },
  methods: {
    toggleBasketVisibility(visibility) {
      this.$emit("toggle-vb", visibility);
    },
    toggleModalVisibility(visibility) {
      this.$emit("toggle-modal", visibility);
    }
  }
};
</script>

<style scoped lang="sass">
.header
  width: 100%
  height: 100px
  background-image: linear-gradient(to bottom, rgb(160, 158, 158), rgb(238, 235, 235) 50%, rgb(150, 148, 148))
  display: flex
.left-wrapper,
.rigth-wrapper
  width: 50%
  height: 100px
  display: flex
  align-items: center
.left-wrapper
  justify-content: start
.rigth-wrapper
  justify-content: space-evenly
.header-logo
  height: auto
  width: 150px
  margin: 0 auto 0 50px
.btn-modal
  width: 200px
.btn-basket
  width: 150px
</style>
