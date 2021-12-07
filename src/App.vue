<script setup lang="ts">
import Card from "./components/Card.vue";
import Web3 from "web3";
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
const provider = ref();
const account = ref();
const library = ref();
const balance = ref();
const message = ref("");
const connect = async () => {
  message.value = "Connecting...";
  provider.value = window.ethereum;
  await provider.value.request({ method: "eth_requestAccounts" });
  account.value = (await provider.value.request({ method: "eth_accounts" }))[0];
  if (!!provider.value) {
    library.value = new Web3(provider.value);
  }
  message.value = "";
};
const getBalance = async () => {
  message.value = "Getting balance...";
  balance.value = await library.value.eth.getBalance(account.value);
  message.value = "";
};
</script>

<template>
  <section
    class="
      bg-gradient-to-br bg-gradient-to-br
      from-purple-300
      to-purple-600
      w-full
      min-h-screen
      flex
      items-center
      justify-center
    "
  >
    <Card class="overflow-hidden">
      <section class="p-3">
        <div class="flex flex-col min-w-128 items-center">
          <div>
            <button @click="connect">Connect Wallet</button>
          </div>
          <table class="font-sans w-full my-2">
            <tr>
              <td class="border border-gray-500 p-2">Account</td>
              <td class="border border-gray-500 p-2 font-mono">
                {{ account || "Not connected" }}
              </td>
            </tr>
            <tr>
              <td class="border border-gray-500 p-2">Balance</td>
              <td class="border border-gray-500 p-2 font-mono">
                {{ balance || "---" }}
              </td>
            </tr>
          </table>
          <div>
            <button @click="getBalance">Get Balance</button>
          </div>
        </div>
      </section>
      <footer class="p-3 text-center bg-gray-600 text-white font-mono">
        {{ message || 'No message' }}
      </footer>
    </Card>
  </section>
</template>
