<template>
  <div class="custom-input">
    <label :for="label">
      <input
          :id="label"
          class="custom-input__input"
          :class="inputClass"
          :type="inputType"
          :value="modelValue"
          @input="handleInput($event)"
      />
    </label>
    <span class="custom-input__label">{{ placeholder }}</span>
    <svg
      v-if="label === 'password'"
      class="custom-input__svg pointer"
      xmlns="http://www.w3.org/2000/svg"
      width="12"
      height="8"
      viewBox="0 0 12 8"
      fill="none"
      @click="changeVisible">
      <path
          d="M11.9655 4.1621C11.9479 4.2016 11.5245 5.1409 10.5828 6.0826C9.32864 7.33695 7.74385 8 6 8C4.25615 8 2.67135 7.337 1.41716 6.08265C0.475458 5.1409 0.0520088 4.20155 0.0344588 4.16205C0.011739 4.11091 0 4.05556 0 3.9996C0 3.94364 0.011739 3.8883 0.0344588 3.83715C0.0520588 3.79765 0.475458 2.85855 1.41721 1.91695C2.6714 0.6629 4.2561 0 6 0C7.7439 0 9.3286 0.6629 10.5828 1.91695C11.5245 2.85855 11.9479 3.7976 11.9655 3.83715C11.9883 3.8883 12 3.94364 12 3.9996C12 4.05556 11.9882 4.11096 11.9655 4.1621ZM6 5.7996C6.35601 5.7996 6.70402 5.69403 7.00002 5.49625C7.29603 5.29846 7.52674 5.01734 7.66298 4.68843C7.79922 4.35952 7.83486 3.9976 7.76541 3.64844C7.69596 3.29927 7.52452 2.97854 7.27279 2.72681C7.02106 2.47507 6.70033 2.30364 6.35116 2.23419C6.002 2.16473 5.64008 2.20038 5.31117 2.33662C4.98226 2.47285 4.70114 2.70356 4.50336 2.99957C4.30557 3.29558 4.2 3.64359 4.2 3.9996C4.20053 4.47683 4.39034 4.93436 4.72779 5.27181C5.06524 5.60926 5.52277 5.79907 6 5.7996Z"
          fill="#403A4B"
          fill-opacity="0.6"/>
    </svg>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    type: {
      type: String,
      default: ""
    },
    label: {
      type: String,
      default: ""
    },
    placeholder: {
      type: String,
      default: ""
    },
    modelValue: {
      type: String,
      default: ""
    },
    errorPassword: {
      type: Boolean,
      default: false
    }
  },
  emits: ["update:modelValue", "inputError"],
  data() {
    return {
      isVisiblePassword: false,
      value: "",
      confirm: false,
    };
  },
  computed: {
    inputType() {
      return this.isVisiblePassword ? "text" : this.type === "password" ? "password" : "email";
    },
    emailValidation(): boolean {
      let txt = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return txt.test(this.modelValue);
    },

    checkInput() {
      const emailError = this.label === "email" && !this.emailValidation;
      const passwordError = this.label === "password" && this.modelValue.length < 6;
      const confirmPasswordError = this.label === "passwordConfirm" && this.errorPassword;
      return { emailError, passwordError, confirmPasswordError }
    },

    inputClass() {
      if (this.checkInput.emailError || this.checkInput.passwordError || this.checkInput.confirmPasswordError) {
        this.confirm = false
        return "error-form";
      } else {
        this.confirm = true
        this.$emit("inputError", this.confirm);
        return "success";
      }
    },
  },

  methods: {
    handleInput($event: any) {
      this.confirm = !(this.checkInput.emailError || this.checkInput.passwordError || this.checkInput.confirmPasswordError);
      this.$emit("update:modelValue", $event.target.value);
      this.$emit("inputError", this.confirm);
    },

    changeVisible() {
      this.isVisiblePassword = !this.isVisiblePassword;
    },
  }
});
</script>
