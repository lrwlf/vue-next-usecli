<template>
  <div class="Hellow">
    <h1> Hellow page {{counter}}</h1>
  </div>
</template>
<script>
import { inject, ref } from 'vue';

export default {
  setup() {
    const counter = inject('counter', ref(999));
    return {
      counter,
    };
  },
};
</script>
