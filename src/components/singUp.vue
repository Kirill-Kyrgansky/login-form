<template>
  <custom-input
      v-model="user.email"
      :placeholder="'E-mail'"
      :label="'email'"
      :type="'email'"
      @input-error="isEmailError"
  />
  <custom-input
      v-if="!isVisiblePassword"
      v-model="user.password"
      :placeholder="'Пароль'"
      :label="'password'"
      :type="'password'"
      @input-error="isPasswordError"
  />
  <custom-input
      v-if="!isVisiblePassword"
      v-model="duplicatePassword"
      :placeholder="'Подтвердите пароль'"
      :error-password="errorPassword"
      :label="'passwordConfirm'"
      :type="'password'"
      @input-error="isPasswordConfirmError"

  />
  <custom-button v-if="!isLoading"  type="submit" @click="singUp">Регистрация</custom-button>
  <loading-spinner v-else/>

</template>

<script lang="ts">
import {defineComponent} from "vue";
import {IUser} from "@/interface/IUser";
import CustomInput from "@/components/UI/customInput.vue";
import CustomButton from "@/components/UI/customButton.vue";
import axios, {AxiosResponse} from "axios";
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
    const isVisiblePassword = false;
    const duplicatePassword = "";
    return {
      user: user,
      isVisiblePassword,
      duplicatePassword,
      lengthPasswordCorrect: false,
      emailError: false,
      passwordError: false,
      passwordConfirmError: false,
      isLoading: false
    };
  },
  computed: {
    errorPassword(): boolean {
      return this.duplicatePassword !== this.user.password
    },
  },
  methods: {
    isEmailError(error: boolean) {
      this.emailError = error;
    },
    isPasswordError(error: boolean) {
      this.passwordError = error;
    },
    isPasswordConfirmError(error: boolean) {
      this.passwordConfirmError = error
    },
    async singUp() {
      if (!this.emailError) {
        alert('Некорректный "E-mail"')
      } else if (!this.passwordError) {
        alert('Некорректный пароль')
      } else if (this.errorPassword) {
        alert('Пароли не совпадают')
      } else  {
        this.isLoading = true
        try {
          const response: AxiosResponse = await axios.post(`https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=${this.apiKey}`, this.user, {});
          if (response.status >= 200 && response.status < 300) {
            alert("Вы успешно зарегистрировались");
            this.isLoading = false
          }
        } catch (err: any) {
          if (err.response && err.response.data && err.response.data.error) {
            if (err.response.data.error.message === "INVALID_EMAIL") {
              alert("Некорректный формат E-mail");
              this.isLoading = false
            }
          }
        }
      }
    }
  }
})
</script>
