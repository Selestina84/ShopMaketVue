<template>
  <div class="feedback-modal" v-if="visibilityModal" @click.self="toggleMD">
    <div class="feedback-modal-container">
      <template v-if="showPreloader">
        <Preloader />
      </template>
      <template v-if="thanksVisibility">
        <div class="thank-wrapper">
          <span class="thank-title">
            Спасибо! Мы свяжемся с вами в ближайшее время!
          </span>
        </div>
      </template>
      <template v-else>
        <button class="delete-btn modal-close-btn" @click="toggleMD">
          &#215;
        </button>
        <form
          action="#"
          class="feedback-form"
          @submit.prevent="sendData($event)"
        >
          <input
            placeholder="Ваше имя"
            name="name"
            type="text"
            class="feedback-input"
            :class="{ 'input-error': $v.name.$error }"
            v-model.trim="$v.name.$model"
          />
          <span v-if="$v.name.$error" class="error">
            <template v-if="!$v.name.minLength">
              В имени должно быть не меньше
              {{ $v.name.$params.minLength.min }} букв.
            </template>
            <template v-else-if="!$v.name.alpha">
              Имя должно содержать только буквы
            </template>
            <template v-else>
              Поле обязательно для заполнения
            </template>
          </span>
          <input
            placeholder="Ваш номер телефона в формате +7(000)000-00-00"
            name="phone"
            type="phone"
            class="feedback-input"
            :class="{ 'input-error': $v.phone.$error }"
            v-model.trim="$v.phone.$model"
          />
          <span v-if="$v.phone.$error" class="error">
            <template v-if="!$v.phone.valid">
              Телефон должен быть в формате +7(000)000-00-00
            </template>
            <template v-else>
              Поле обязательно для заполнения
            </template>
          </span>
          <input
            required
            placeholder="Ваш e-mail"
            name="email"
            type="e-mail"
            class="feedback-input"
            :class="{ 'input-error': $v.email.$error }"
            v-model.trim="$v.email.$model"
          />
          <span v-if="$v.email.$error" class="error">
            <template v-if="!$v.email.email">
              Введите действующий e-mail
            </template>
            <template v-else>
              Поле обязательно для заполнения
            </template>
          </span>
          <textarea
            placeholder="Ваше сообщение"
            name="message"
            id="message"
            cols="30"
            rows="5"
            class="feedback-text"
            v-model="message"
          ></textarea>
          <button class="btn-main btn-feedback" :disabled="$v.$invalid">
            Связаться со мной
          </button>
        </form>
      </template>
    </div>
  </div>
</template>

<script>
import Preloader from "@/components/Preloader";
import { required, minLength, email } from "vuelidate/lib/validators";
export default {
  name: "Modal",
  props: ["visibilityModal", "API"],
  components: {
    Preloader
  },
  data: () => ({
    id: Math.round(Math.random() * 1e9),
    name: "",
    phone: "",
    email: "",
    message: "",
    showPreloader: false,
    thanksVisibility: false,
    user: {}
  }),
  validations: {
    name: {
      required,
      minLength: minLength(2),
      alpha: val => /^[а-яёa-z]*$/i.test(val)
    },
    phone: {
      required,
      validFormat: val => /^\+7+\(+\d{3}\)+\d{3}-\d{2}-\d{2}$/.test(val)
    },
    email: {
      required,
      email
    }
  },
  methods: {
    newForm() {
      this.modalState = Object.assign({}, this.modalDefaultState);
      return this.modalState;
    },
    toggleMD(visibility) {
      this.$emit("toggle-modal", visibility);
    },

    createUser() {
      (this.user.id = this.id),
        (this.user.name = this.name),
        (this.user.phone = this.phone),
        (this.user.email = this.email),
        (this.user.message = this.message);
      return this.user;
    },
    showThank() {
      this.thanksVisibility = true;
      setTimeout(() => {
        this.toggleMD(this.visibilityModal);
        this.thanksVisibility = false;
      }, 1500);
    },
    sendData(event) {
      (this.showPreloader = true),
        fetch(`${this.API}/users`, {
          method: "POST",
          body: JSON.stringify(this.createUser()),
          headers: {
            "Content-type": "application/json"
          }
        })
          .then(response => response.json())
          .then(json => {
            event.target.reset();
            console.log(json);
          })
          .catch(error => console.log(error))
          .finally(() => {
            this.showPreloader = false;
            this.showThank();
          });
    }
  }
};
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

.thank-wrapper
  padding: 10px
  display: flex
  justify-content: center
  align-items: center
  font-weight: bold
  color: rgb(87, 85, 85)
  font-size: 22px
</style>
