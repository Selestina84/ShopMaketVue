<template>
  <div class="search">
    <form action="#" class="search-form">
        <input type="text" class="search-input" v-model.trim="search" @input ="$parent.$emit('search-text', search)">
        <button type="submit" class="btn-search" @click="$parent.$emit('search-text', search)"></button>
    </form>
  </div>
</template>

<script>
export default {
  name: "Search",
  data: () => ({
    search: '',
  })

}
</script>

<style lang="sass" scoped>
.search-form
  display: flex
  align-items: center
.btn-search
  width: 50px
  height: 50px
  background: url(./../assets/searchBtn.png)center center/cover
  border: none
  &:hover
    cursor: pointer
    transform: scale(1.05)
  &:focus
    outline: none
.search-input
  height: 50px
  background-color: transparent
  border: none
  outline: none
  border-bottom: 2px solid rgb(87, 85, 85)
  font-size: 20px
  font-weight: bold
  color: rgb(87, 85, 85)
</style>