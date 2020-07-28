<template>
  <div class="about">
    <h1>This is an about page {{counter}}</h1>
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
