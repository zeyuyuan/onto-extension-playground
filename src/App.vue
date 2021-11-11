<template>
  <n-notification-provider>
    <Ethereum />
  </n-notification-provider>
</template>

<script setup>
import { NNotificationProvider } from "naive-ui";
import Ethereum from "./views/ethereum.vue";
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
