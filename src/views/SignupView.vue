<template>
  <div class="signup">
    <PassInput v-model="pin" placeholder="Enter 6 Digit Pin" />
    <PassInput v-model="rePin" placeholder="Re-Enter Pin" />
    <Button @click="register" />
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import Button from "@/components/Button.vue";
import PassInput from "../components/PassInput.vue";
import { Preferences } from "@capacitor/preferences";
import router from "@/router";

let pin = ref("");
let rePin = ref("");

const register = async () => {
  console.log(pin, rePin);
  if (pin.value === rePin.value && pin.value.length === 6) {
    await Preferences.set({ key: "pin", value: pin.value });
    router.push("/login");
  } else {
    alert("failed" + pin + " " + repin);
  }
};

onMounted(async () => {
  const pin = await Preferences.get({ key: "pin" });
  if (pin.value) {
    router.push("/login");
  }
});
</script>

<style lang="scss" scoped>
.signup {
  width: 100%;
  height: 100%;

  display: flex;
  justify-content: center;
  align-items: center;

  flex-direction: column;
  background: #141414;
}
</style>
