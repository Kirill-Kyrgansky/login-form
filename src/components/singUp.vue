<template>
  <custom-input
      v-model="user.email"
      :placeholder="'E-mail'"
      :label="'email'"
      :type="'email'"

  />
  <div>
    <custom-input
        v-if="!isVisiblePassword"
        v-model="user.password"
        :placeholder="'Пароль'"
        :label="'password'"
        :type="'password'"
    />
    <custom-input
        v-if="!isVisiblePassword"
        v-model="duplicatePassword"
        :placeholder="'Подтвердите пароль'"
        :error-password="errorPassword"
        :label="'passwordConfirm'"
        :type="'password'"
    />
  </div>
  <custom-button type="submit" @click="singUp">Регистрация</custom-button>
  <forgot-password/>
</template>

<script lang="ts">
import {defineComponent} from "vue";
import axios from "axios";
import {IUser} from "@/interface/IUser";
import CustomInput from "@/components/UI/customInput.vue"
import CustomButton from "@/components/UI/customButton.vue"
import ForgotPassword from "@/components/UI/forgotPassword.vue";

export default defineComponent({
  components: {ForgotPassword, CustomInput, CustomButton},
  props: {
    apiKey: {
      type: String,
      default: ''
    }
  },
  data() {
    const user: IUser = {
      email: "",
      password: "",
      returnSecureToken: true
    };
    const isVisiblePassword = false;
    const duplicatePassword = '';
    return {
      user: user,
      isVisiblePassword,
      duplicatePassword,
      lengthPasswordCorrect: false
    };
  },
  computed: {
    errorPassword(): boolean {
      return this.duplicatePassword !== this.user.password
    },
  },
  methods: {
    async singUp() {
      await axios.post(`https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=${this.apiKey}`, this.user)
          .then(value => {
            document.cookie = `idToken=${value.data.idToken}`
            document.cookie = `refreshToken=${value.data.refreshToken}`
            console.log(value)
          })
          .catch(err => {
            console.error(err)
          })
    },
  }
})
</script>

<style scoped>

</style>
