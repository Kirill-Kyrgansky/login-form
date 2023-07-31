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
  <custom-button type="submit" @click="singUp">Регистрация</custom-button>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";
import { IUser } from "@/interface/IUser";
import CustomInput from "@/components/UI/customInput.vue";
import CustomButton from "@/components/UI/customButton.vue";
import { AxiosResponse } from "axios/index";

export default defineComponent({
  components: { CustomInput, CustomButton },
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
    let emailError = false;
    let passwordError = false;
    let passwordConfirmError = false;
    return {
      user: user,
      isVisiblePassword,
      duplicatePassword,
      lengthPasswordCorrect: false,
      emailError,
      passwordError,
      passwordConfirmError
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
      console.log(error);
    },
    isPasswordError(error: boolean) {
      this.passwordError = error;
      console.log(error);

    },
    isPasswordConfirmError(error: boolean) {
      this.passwordConfirmError = error
      console.log(error);

    },
    async singUp() {
      console.log(this.passwordConfirmError);
      console.log(this.passwordError);
      console.log(this.emailError);
      // if (this.user.password.length < 6) {
      //   alert("Пароль не может быть меньше 6 символов");
      // } else if (this.user.email.length === 0) {
      //   alert("Поле \"E-mail\" не может быть пустым");
      // } else {
      //   try {
      //     const response: AxiosResponse = await axios.post(`https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=${this.apiKey}`, this.user, {});
      //     if (response.status >= 200 && response.status < 300) {
      //       alert("Вы успешно зарегистрировались");
      //     }
      //   } catch (err: any) {
      //     if (err.response && err.response.data && err.response.data.error) {
      //       if (err.response.data.error.message === "INVALID_EMAIL") {
      //         alert("Некорректный формат E-mail");
      //       }
      //     }
      //   }
      // }
    }
  }
})
</script>
