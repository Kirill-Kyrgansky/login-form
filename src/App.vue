<template>
  <section class="section ">
    <div class="container form-wrap">
      <div>
        <div class=" link">
          <p
            class="pointer"
            :class="{'strong': !isSingUp}"
            @click="changeForm(false)">Вход</p>
          <p
            class="pointer"
            :class="{'strong': isSingUp}"
            @click="changeForm(true)">Регистрация</p>
        </div>
        <form @submit.prevent>
          <sing-up
            v-if="isSingUp"
            :api-key="apiKey" />
          <sing-in
            v-else
            :api-key="apiKey" />
        </form>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import SingIn from "@/components/singIn.vue";
import SingUp from "@/components/singUp.vue";


export default defineComponent({
  components: {SingUp, SingIn},
  data() {
    return {
      isSingUp: false,
      apiKey: 'AIzaSyAjNfkPGxW7MblOHBEgq7Kf4WwxnQdFAaY'
    }
  },
  methods: {
    changeForm(value: boolean) {
      this.isSingUp = value
    }
  }
});
</script>

<style lang="scss">

</style>
