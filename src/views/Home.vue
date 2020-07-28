<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld/>
    <div>{{counter}}</div>
    <button @click="addCount">+1</button>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';
import {
  ref,
  provide,
} from 'vue';

export default {
  name: 'Home',
  components: {
    HelloWorld,
  },
  setup() {
    const counter = ref(0);
    provide('counter', counter);
    function addCount() {
      counter.value += 1;
    }
    return {
      counter,
      addCount,
    };
  },

};
</script>
