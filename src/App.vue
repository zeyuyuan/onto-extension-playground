<template>
  <n-notification-provider>
    <RouterView />
  </n-notification-provider>
</template>

<script setup>
import { NNotificationProvider } from "naive-ui";
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

ul {
  margin: 0;
}
</style>
