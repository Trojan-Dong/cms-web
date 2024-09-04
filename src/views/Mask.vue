<!-- Mask.vue -->
<template>
  <div v-if="visible" class="mask"></div>
</template>

<script>
export default {
  props: ['visible']
};
</script>

<style scoped>
.mask {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: black;
  z-index: 9999;
}
</style>
