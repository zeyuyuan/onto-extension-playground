<template>
  <n-list-item>
    <n-thing title="enable" title-extra="Legacy api">
      <n-button @click="testEnable">Test enable()</n-button>
      <span>{{ result }}</span>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { NListItem, NThing, NButton } from "naive-ui";
import { inject, ref } from "vue";

const provider = inject("provider");
const result = ref("");

const testEnable = async () => {
  result.value = await provider.value.enable();
};
</script>
