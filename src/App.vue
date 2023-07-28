<template>
  <section class="section">
    <div class="container">
      <form @submit.prevent>
        <custom-input
          v-model="user.login"
          :placeholder="'Логин'"
          :label="'login'"
          :type="'text'"
        />
        <div>
          <custom-input
            v-if="!isVisiblePassword"
            v-model="user.password"
            :placeholder="'Пароль'"
            :label="'password'"
            :type="'password'"
          />
        </div>
        <div>

        </div>
        <button type="submit">Войти</button>
      </form>
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import CustomInput from "@/components/customInput.vue";
import { IUser } from "@/interface/IUser";


export default defineComponent({

  components: { CustomInput },
  emits: ["type"],
  data() {
    const user: IUser = {
      login: "",
      password: ""
    };
    const isVisiblePassword: boolean = false;
    return {
      user: user,
      isVisiblePassword: isVisiblePassword
    };
  },
  methods: {
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
  max-width: 460px;
  @include horizontal-center();
  border-radius: 30px;
  background: #FFF;
  box-shadow: 0 4px 17px 0 rgba(0, 0, 0, 0.17);
  padding: 90px;
}
</style>
