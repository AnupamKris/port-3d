<template>
  <div class="split">
    <div class="char" v-for="(char, index) in text" :key="index">
      {{ char }}
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  text: String,
});
</script>

<style lang="scss" scoped>
.split {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
