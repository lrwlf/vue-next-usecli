<template>
  <div class="about">
    <h1>鼠标位置为：({{x}},{{y}})</h1>
  </div>
</template>
<script>
import useMouse from '../utils/mouse';

export default {
  setup() {
    const { x, y } = useMouse();
    return {
      x,
      y,
    };
  },
};
</script>
