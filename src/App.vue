<script setup>
import VueCalzyr from './VueCalzyr.vue';
</script>

<template>
  <!--  Light mode example-->
  <VueCalzyr showOption showDayOfWeek/>
  <div id="padding-top">
    <!--  Dark mode example-->
    <VueCalzyr darkMode highlightToday/>
  </div>
</template>

<style scoped>
#padding-top {
  padding-top: 50px;
}
</style>
