<template>
    <button :type="type" class="button-wrap__button"><slot></slot></button>
</template>

<script>
import {defineComponent} from "vue";

export default defineComponent({
  props: {
    type: {
      type: String,
      default: ""
    },
  }
})
</script>

<style scoped>
button {
  border: none;
}
.button-wrap__button {
  color: #FFF;
  text-align: center;
  font-size: 12px;
  font-style: normal;
  font-weight: 700;
  line-height: normal;
  text-transform: uppercase;
  border-radius: 7px;
  background: #985ACE;
  width: 100%;
  padding: 15px 15px;
}
</style>
