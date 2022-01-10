<template>
  <n-list-item>
    <n-thing title="hex to number">
      <input type="text" v-model="hex" />
      <n-button @click="hex2Number">hex to number</n-button>
      <div>{{ hexResult || "-" }}</div>
    </n-thing>
  </n-list-item>
  <n-list-item>
    <n-thing title="wei to ether count">
      <input type="text" v-model="wei" />
      <n-button @click="wei2Count">wei to count</n-button>
      <div>{{ count || "-" }}</div>
    </n-thing>
  </n-list-item>
  <n-list-item>
    <n-thing title="number to hex">
      <input type="text" v-model="number2" />
      <div>{{ number2InHex || "-" }}</div>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { NListItem, NThing, NButton } from "naive-ui";
import { computed, ref } from "vue";
import Web3 from "web3";

const hex = ref("");
const hexResult = ref("");

const hex2Number = () => {
  try {
    hexResult.value = Web3.utils.hexToNumberString(hex.value);
  } catch (e) {
    hexResult.value = String(e.message);
  }
};

const wei = ref("");
const count = ref("");

const wei2Count = () => {
  count.value = Web3.utils.fromWei(wei.value);
};

const number2 = ref("");
const number2InHex = computed(() => {
  return Web3.utils.numberToHex(number2.value || "0");
});
</script>
