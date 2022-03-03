<template>
  <n-notification-provider>
    <div class="layout">
      <n-list bordered>
        <n-list-item>
          <n-thing title="test connect">
            <n-button @click="connect">Connect</n-button>
          </n-thing>
        </n-list-item>
        <n-list-item>
          <n-thing title="test send ont">
            <n-button @click="sendOnt">Send ONT</n-button>
          </n-thing>
        </n-list-item>
      </n-list>
    </div>
  </n-notification-provider>
</template>

<script setup>
import {
  NList,
  NListItem,
  NThing,
  NButton,
  // NSpace,
  // NRadio,
  // NRadioGroup,
  // NGrid,
  // NGridItem,
  // useNotification,
  NNotificationProvider,
} from "naive-ui";
import { client, ExtensionType } from "@ont-dev/ontology-dapi";
import { ref } from "vue";

const ontoClient = ref(null);
const account = ref("");

const connect = async () => {
  ontoClient.value = client.registerClient({
    logMessages: true,
    logWarnings: true,
    extension: ExtensionType.Cyano,
  });
  account.value = await ontoClient.value.asset.getAccount();
};

const sendOnt = async () => {
  if (!ontoClient.value) {
    throw new Error("connect first");
  }
  const re = await ontoClient.value.network.getBalanceV2({
    address: account.value,
  });
  console.log(re);
  const result = await ontoClient.value.asset.sendV2({
    to: "AavQvmUXJBebmpahnXbXMu1cs1JuFtCYkd",
    asset: "ONG",
    amount: String(2),
    // amount: "100000000000000000",
  });
  console.log("result", result);
};
</script>

<style lang="scss">
.layout {
  max-width: 800px;
  margin: 50px auto;
}
</style>
