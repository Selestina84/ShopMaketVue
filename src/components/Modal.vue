<template>
   <div class="feedback-modal" v-if="visibilityModal">
      <div class="feedback-modal-container">
          <h2 class="feedback-title">Мы свяжемся с Вами в ближайшее время</h2>
          <button class="delete-btn modal-close-btn" @click="toggleMD">x</button>
          <form action="#" class="feedback-form">
            <input required placeholder="Ваше имя" name="name" type="text" class="feedback-input">
            <span class="validation ">Имя должно содержать только буквы</span>
            <input required placeholder="Ваш номер телефона" name="phone" type="phone" class="feedback-input">
            <span class="validation ">Телефон должен быть в формате +7(000)000-00-00</span>
            <input required placeholder="Ваш e-mail" name="email" type="e-mail" class="feedback-input">
            <span class="validation">E-mail должен быть в формате mymail@mail.ru, или my.mail@mail.ru, или my-mail@mail.ru</span>
            <textarea placeholder="Ваше сообщение" name="message" id="message" cols="30" rows="5" class="feedback-text"></textarea>
            <button class="btn-feedback disabled" disabled>Связаться со мной</button>
          </form>
      </div>
    </div>
</template>

<script>
export default {
  name: "Modal",
  props: ["visibilityModal"],
  methods: {
    toggleMD(visibility){
      this.$emit('toggle-modal', visibility)
    }
  }

}
</script>

<style lang="sass">
.feedback-modal
  width: 100%
  height: 100vh
  position: absolute
  z-index: 1
  background-color: rgba(0, 0, 0, 0.5)
  display: flex
  justify-content: center
  align-items:center
.feedback-modal-container
  width: 500px
  height: 400px
  background-color: rgb(204, 201, 201)
  padding: 20px

.feedback-title
  display: inline-block
  margin: 0 30px 20px 20px
  font-size: 20px
  font-weight: bold
  text-align: center
  color: rgb(87, 85, 85)

.feedback-form
  width: 70%
  margin: 0 auto
  display: flex
  flex-direction: column

.feedback-input
  margin: 0 0 10px 0
  padding: 5px
  box-sizing: border-box
  height: 35px
  font-size: 15px
  font-weight: bold
  color: rgb(87, 85, 85)

.feedback-text
  margin: 0 0 20px 0
  padding: 5px
  box-sizing: border-box
  font-size: 17px
  font-weight: bold
  color: rgb(87, 85, 85)
  resize: none

.btn-feedback
  width: 60%
  height: 40px
  margin: 0 auto 20px
  background:  linear-gradient(to bottom, rgb(110, 109, 109), rgb(238, 235, 235) 50%, rgb(105, 105, 105))
  font-weight: bold
  color: rgb(61, 60, 60)
  font-size: 18px

.validation
  display: none
  margin: 0 0 10px 0
  text-align: center
  font-size: 15px
  color: red

.disabled
  opacity: 0.5

.invalid
  outline: 1px solid red
</style>