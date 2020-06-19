<template>
  <div class="feedback-modal" v-if="visibilityModal">
    <div class="feedback-modal-container">
      <h2 class="feedback-title">Мы свяжемся с Вами в ближайшее время</h2>
        <button class="delete-btn modal-close-btn" @click="toggleMD">x</button>
          <form action="#" class="feedback-form" @submit.prevent="sendData">
            <input placeholder="Ваше имя" name="name" type="text" class="feedback-input" v-model.trim="$v.name.$model">
            <span v-if="$v.name.$error" class="error">
              <template v-if="!$v.name.minLength">
                  В имени должно быть не меньше {{$v.name.$params.minLength.min}} букв.
              </template>
              <template v-else-if="!$v.name.alpha">
                  Имя должно содержать только буквы
              </template>
              <template v-else>
                  Поле обязательно для заполнения
              </template>
            </span>
            <input placeholder="Ваш номер телефона в формате +7(000)000-00-00" name="phone" type="phone" class="feedback-input" v-model.trim="$v.phone.$model">
            <span v-if="$v.phone.$error" class="error">
              <template v-if="!$v.phone.numeric">
                  Телефон должен быть в формате +7(000)000-00-00
              </template>
              <template v-else>
                  Поле обязательно для заполнения
              </template>
            </span>
            <input required placeholder="Ваш e-mail" name="email" type="e-mail" class="feedback-input" v-model.trim="$v.email.$model">
            <span v-if="$v.email.$error" class="error">
              <template v-if="!$v.email.email">
                  Введите действующий e-mail
              </template>
              <template v-else>
                  Поле обязательно для заполнения
              </template>
            </span>
            <textarea placeholder="Ваше сообщение" name="message" id="message" cols="30" rows="5" class="feedback-text" v-model="message"></textarea>
            <button class="btn-feedback" :disabled="$v.$invalid">Связаться со мной</button>
          </form>
      </div>
  </div>
</template>

<script>
import { required, minLength, email} from 'vuelidate/lib/validators';
export default {
  name: "Modal",
  props: ["visibilityModal","API"],
  data:() => ({
    id: Math.round(Math.random()*1e9),
    name: '',
    phone: '',
    email: '',
    message: '',
    user: {}
  }),
  validations: {
    name: {
      required,
      minLength: minLength(2),
      alpha: val => /^[а-яёa-z]*$/i.test(val),

    },
    phone: {
      required,
      validFormat: val => /^\+7+\(+\d{3}\)+\d{3}-\d{2}-\d{2}$/.test(val),
    },
    email: {
      required,
      email
    }
  },
  methods: {
    toggleMD(visibility){
      this.$emit('toggle-modal', visibility)
    },

    createUser(){
      this.user.id = this.id,
      this.user.name= this.name,
      this.user.phone= this.phone,
      this.user.email= this.email,
      this.user.message=this.message
      return this.user
      },
    sendData(){
      fetch(`${this.API}/users`, {
            method: "POST",
            body: JSON.stringify(this.createUser()),
            headers: {
                'Content-type': 'application/json'
            }
        })
        .then(response => response.json())
        .then(json => console.log(json));
      }
    }
  }
</script>

<style lang="sass">
.feedback-modal
  width: 100%
  height: 100%
  position: absolute
  z-index: 1
  background-color: rgba(0, 0, 0, 0.5)
  display: flex
  justify-content: center
  align-items:center

.feedback-modal-container
  width: 600px
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

.btn-feedback[disabled]
  opacity: 0.5

.error
  margin: 0 0 10px 0
  color: red
</style>