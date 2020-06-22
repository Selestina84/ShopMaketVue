<template>
  <div class="feedback-modal" v-if="visibilityModal" @click.self="toggleMD">
    <div class="feedback-modal-container">
      <template v-if ="showPreloader">
        <div class="preloader">
          <svg class="preloader__image" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
           <path fill="currentColor"
            d="M304 48c0 26.51-21.49 48-48 48s-48-21.49-48-48 21.49-48 48-48 48 21.49 48 48zm-48 368c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48-21.49-48-48-48zm208-208c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48-21.49-48-48-48zM96 256c0-26.51-21.49-48-48-48S0 229.49 0 256s21.49 48 48 48 48-21.49 48-48zm12.922 99.078c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48c0-26.509-21.491-48-48-48zm294.156 0c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48c0-26.509-21.49-48-48-48zM108.922 60.922c-26.51 0-48 21.49-48 48s21.49 48 48 48 48-21.49 48-48-21.491-48-48-48z">
           </path>
         </svg>
        </div>
      </template>
      <template v-if="thanksVisibility">
        <div class="thank-wrapper">
          <span class="thank-title">
            Спасибо! Мы свяжемся с вами в ближайшее время!
          </span>
        </div>
      </template>
      <template v-else>
        <button class="delete-btn modal-close-btn" @click="toggleMD">&#215;</button>
        <form action="#" class="feedback-form" @submit.prevent="sendData($event)">
          <input placeholder="Ваше имя" name="name" type="text" class="feedback-input" :class="{ 'input-error': $v.name.$error }" v-model.trim="$v.name.$model">
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
          <input placeholder="Ваш номер телефона в формате +7(000)000-00-00" name="phone" type="phone" class="feedback-input" :class="{ 'input-error': $v.phone.$error }" v-model.trim="$v.phone.$model">
          <span v-if="$v.phone.$error" class="error">
            <template v-if="!$v.phone.valid">
                Телефон должен быть в формате +7(000)000-00-00
            </template>
            <template v-else>
                Поле обязательно для заполнения
            </template>
          </span>
          <input required placeholder="Ваш e-mail" name="email" type="e-mail" class="feedback-input" :class="{ 'input-error': $v.email.$error }" v-model.trim="$v.email.$model">
          <span v-if="$v.email.$error" class="error">
            <template v-if="!$v.email.email">
                Введите действующий e-mail
            </template>
            <template v-else>
                Поле обязательно для заполнения
            </template>
          </span>
          <textarea placeholder="Ваше сообщение" name="message" id="message" cols="30" rows="5" class="feedback-text" v-model="message"></textarea>
          <button class="btn-main btn-feedback" :disabled="$v.$invalid">Связаться со мной</button>
        </form>
      </template>
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
    showPreloader: false,
    thanksVisibility: false,
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
    newForm(){
      this.modalState = Object.assign({}, this.modalDefaultState);
      return this.modalState
    },
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
    showThank(){
      this.thanksVisibility = true;
      setTimeout(() => {
        this.toggleMD(this.visibilityModal)
        this.thanksVisibility = false;
        }, 2000);
    },
    sendData(event){
      this.showPreloader = true,
      fetch(`${this.API}/users`, {
            method: "POST",
            body: JSON.stringify(this.createUser()),
            headers: {
                'Content-type': 'application/json'
            }
        })
        .then(response => response.json())
        .then(json =>{
        event.target.reset()
        console.log(json)})
        .finally(() => {
        this.showPreloader = false
        this.showThank()
        });
    }
  }
  }
</script>

<style lang="sass" scoped>
.feedback-modal
  width: 100%
  height: 100vh
  position: absolute
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1
  background-color: rgba(0, 0, 0, 0.5)

.feedback-modal-container
  background: linear-gradient(to bottom, rgb(110, 109, 109), rgb(238, 235, 235) 50%, rgb(105, 105, 105))
  position: relative
  width: 600px
  height: auto
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

.modal-close-btn
  margin: 0 10px 20px auto
  display: block
  background-color: red
  width: 30px
  height: 30px
  font

.btn-feedback[disabled]
  opacity: 0.5
  &:hover
    cursor: default
    transform: none
input
  &:focus
    outline: none

.error
  padding: 0 0 5px 0
  color: red

.input-error
  outline: 1px solid red
  border: none
  &:focus
    outline: 1px solid red

.preloader
  position: absolute
  left: 0
  top: 0
  right: 0
  bottom: 0
  overflow: hidden
  background: #e0e0e0;
  z-index: 1;

.preloader__image
  position: relative
  top: 50%
  left: 50%
  width: 70px
  height: 70px
  margin-top: -35px
  margin-left: -35px
  text-align: center
  animation: preloader-rotate 2s infinite linear

@keyframes preloader-rotate
  100%
    transform: rotate(360deg)

.thank-wrapper
  padding: 10px
  display: flex
  justify-content: center
  align-items: center
  font-weight: bold
  color: rgb(87, 85, 85)
  font-size: 22px
</style>