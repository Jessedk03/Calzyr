<script setup>
import VueCalzyr from './VueCalzyr.vue';
</script>

<template>

  <VueCalzyr showOption showDayOfWeek />
  <div id="padding-top">
    <VueCalzyr darkMode />
  </div>
</template>

<style scoped>
#padding-top {
  padding-top: 50px;
}
</style>
