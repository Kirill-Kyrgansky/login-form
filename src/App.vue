<template>
  <section class="section form-wrap">
    <div class="container">
      <div class="container link">
        <p class="pointer" :class="{'strong': !isSingUp}" @click="changeForm(false)">Вход</p>
        <p class="pointer" :class="{'strong': isSingUp}" @click="changeForm(true)">Регистрация</p>
      </div>
      <form @submit.prevent>
        <sing-up v-if="isSingUp" :api-key="apiKey"/>
        <sing-in v-else :api-key="apiKey"/>
      </form>
    </div>
  </section>
</template>

<script lang="ts">
import {defineComponent} from "vue";
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
@mixin horizontal-center {
  display: flex;
  justify-content: center;
}

$main-color: #985ACE;

body {
  padding: 50px 0 70px 0;
  position: relative;
  background-color: $main-color;
}

.section {
  padding: 0 30px;
  max-width: 1440px;
  margin: auto;
  position: relative;
  @include horizontal-center();

}

.container {
}

.form-wrap {
  border-radius: 30px;
  background: #FFF;
  box-shadow: 0 4px 17px 0 rgba(0, 0, 0, 0.17);
  padding: 90px;
  max-width: 460px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.strong {
  font-weight: bold;
}

.pointer {
  cursor: pointer;
}

.link {
  display: flex;
  justify-content: space-between;

}
</style>
