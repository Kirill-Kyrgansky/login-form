<template>
  <custom-input
      v-model="user.email"
      :placeholder="'E-mail'"
      :label="'email'"
      :type="'email'"
      @input-error="isEmailError"
  />
  <div>
    <custom-input
        v-if="!isVisiblePassword"
        v-model="user.password"
        :placeholder="'Пароль'"
        :label="'password'"
        :type="'password'"
        @input-error="isPasswordError"
    />
  </div>
  <custom-button v-if="!isLoading" type="submit" @click="singIn">Войти</custom-button>
  <loading-spinner v-else/>
</template>

<script lang="ts">
import {defineComponent} from "vue";
import axios, {AxiosResponse} from "axios";
import {IUser} from "@/interface/IUser";
import CustomInput from "@/components/UI/customInput.vue";
import CustomButton from "@/components/UI/customButton.vue";
import LoadingSpinner from "@/components/UI/loadingSpinner.vue";

export default defineComponent({
  components: {LoadingSpinner, CustomInput, CustomButton},
  props: {
    apiKey: {
      type: String,
      default: ""
    }
  },
  data() {
    const user: IUser = {
      email: "",
      password: "",
      returnSecureToken: true
    };
    return {
      user: user,
      emailError: false,
      passwordError: false,
      isVisiblePassword: false,
      isLoading: false
    };
  },

  methods: {
    isEmailError(error: boolean) {
      this.emailError = error;
    },
    isPasswordError(error: boolean) {
      this.passwordError = error;
    },
    async singIn() {
      if (!this.emailError) {
        alert('Некорректный "E-mail"')
      } else if (!this.passwordError) {
        alert('Некорректный пароль')
      } else {
        this.isLoading = true
        try {
          const response: AxiosResponse = await axios.post(`https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key=${this.apiKey}`, this.user, {});
          if (response.status >= 200 && response.status < 300) {
            alert("Вы успешно вошли в систему");
            this.isLoading = false
          }
        } catch (err: any) {
          if (err.response && err.response.data && err.response.data.error) {
            this.isLoading = false
            if (err.response.data.error.message === "INVALID_EMAIL") {
              alert("Некорректный формат E-mail");
            } else if (err.response.data.error.message === "EMAIL_NOT_FOUND") {
              alert("Вы не зарегистрированы в системе");
            }
          }
        }
      }
    }
  }

})
</script>
