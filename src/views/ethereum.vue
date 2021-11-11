<template>
  <n-notification-provider>
    <div class="layout">
      <n-list bordered>
        <n-list-item>
          <n-thing title="Listen connect status...">
            <n-grid :x-gap="30" :cols="7">
              <n-grid-item :span="1">Account:</n-grid-item>
              <n-grid-item :span="6">{{
                account || "not connected"
              }}</n-grid-item>
              <n-grid-item :span="1">ChainId:</n-grid-item>
              <n-grid-item :span="6">{{
                chainId || "not connected"
              }}</n-grid-item>
              <n-grid-item :span="1">Events:</n-grid-item>
              <n-grid-item :span="6">
                <div v-for="(item, index) in events" :key="index">
                  {{ `(${item.provider})${item.type}:${item.value}` }}
                </div>
              </n-grid-item>
            </n-grid>
          </n-thing>
        </n-list-item>
        <n-list-item>
          <n-thing title="Select provider source">
            <n-radio-group v-model:value="providerType" name="providerGroup">
              <n-space>
                <n-radio value="window.ethereum">window.ethereum</n-radio>
                <n-radio value="window.onto">window.onto</n-radio>
                <n-radio value="window.web3">window.web3</n-radio>
                <n-radio value="Web3.givenProvider">Web3.givenProvider</n-radio>
              </n-space>
              <n-button @click="logProvider">log</n-button>
            </n-radio-group>
          </n-thing>
        </n-list-item>
        <n-list-item>
          <n-thing title="Select SDK type">
            <n-radio-group v-model:value="sdkType" name="sdkGroup">
              <n-space>
                <n-radio value="window.ethereum">window.ethereum</n-radio>
                <n-radio value="ethers">ethers</n-radio>
                <n-radio value="web3">web3</n-radio>
              </n-space>
            </n-radio-group>
          </n-thing>
        </n-list-item>
        <RequestAccounts />
        <Enable />
        <ChainId />
      </n-list>
    </div>
  </n-notification-provider>
</template>

<script setup>
import {
  NList,
  NListItem,
  NThing,
  NSpace,
  NRadio,
  NRadioGroup,
  NGrid,
  NGridItem,
  useNotification,
  NNotificationProvider,
  NButton,
} from "naive-ui";
import { onMounted, provide, ref, watch, computed } from "vue";
import ChainId from "../components/ChainId.vue";
import Enable from "../components/Enable.vue";
import RequestAccounts from "../components/RequestAccounts.vue";
import Web3 from "web3";
import { useLocalStorage } from "@vueuse/core";

const getProvider = (providerType) => {
  const map = {
    "window.ethereum": window.ethereum,
    "window.onto": window.onto,
    "window.web3": window.web3,
    "Web3.givenProvider": Web3.givenProvider,
  };
  return map[providerType];
};
console.log(Web3.utils.numberToHex(1));
const providerType = useLocalStorage("provider-type", "window.ethereum");
const sdkType = useLocalStorage("sdk-type", "window.ethereum");
const provider = computed(() => getProvider(providerType.value));
const account = ref("");
const chainId = ref("");
const events = ref([]);
const notification = useNotification();

provide("provider", provider);

const addEvent = (type, value, providerType) => {
  events.value.push({
    type,
    value: JSON.stringify(value),
    provider: providerType,
  });
};

const listenConnect = () => {
  console.log("start listen");
  const currentProvider = provider.value;
  const type = providerType.value;
  if (!currentProvider) {
    notification.error({
      title: "Error",
      content: "Provider not found",
    });
  }
  currentProvider.on("connect", (info) => {
    addEvent("connect", info, type);
  });
  currentProvider.on("accountsChanged", (accounts) => {
    addEvent("accountsChanged", accounts, type);
    account.value = accounts[0];
  });
  currentProvider.on("chainChanged", (chain) => {
    addEvent("chainChanged", chain, type);
    chainId.value = chain;
  });
  currentProvider.on("disconnect", (err) => {
    addEvent("disconnect", err, type);
    account.value = "";
    chainId.value = "";
  });
};

watch(
  () => provider.value,
  () => {
    listenConnect();
  }
);

const logProvider = () => {
  console.log(provider.value);
};

onMounted(() => {
  listenConnect();
});
</script>

<style lang="scss">
.layout {
  max-width: 800px;
  margin: 50px auto;
}
</style>
