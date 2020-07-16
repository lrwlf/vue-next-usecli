<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <div>{{counter}}</div>
    <button @click="addCount">+1</button>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue';
import {
  ref,
} from 'vue';

export default {
  name: 'Home',
  // components: {
  //   HelloWorld,
  // },
  setup() {
    const counter = ref(0);

    const addCount = function () {
      counter.value += 1;
    };
    return {
      counter,
      addCount,
    };
  },

};
</script>
