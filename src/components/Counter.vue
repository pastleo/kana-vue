<template>
  <button v-on:click="cntPlus">cnt: {{ cnt }}</button>
</template>
<script>
import { ref } from 'vue';
export default {
  name: 'Hello',
  setup(/* props */) {
    const cnt = ref(0);
    const cntPlus = () => {
      cnt.value++
    }
    return {
      cnt, cntPlus
    }
  }
}
</script>
