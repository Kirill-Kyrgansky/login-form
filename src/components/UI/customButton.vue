<template>
  <button
      :type="type"
      class="button-wrap__button pointer">
    <slot></slot>
  </button>
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
<style>

</style>
