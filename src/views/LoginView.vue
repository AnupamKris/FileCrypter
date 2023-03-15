<template>
  <div class="signup">
    <PassInput
      v-model="pin"
      placeholder="Enter 6 Digit Pin"
      @input="checkPass"
      :playShake="playShake"
      :playSuccess="playSuccess"
    />
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import Button from "@/components/Button.vue";
import PassInput from "../components/PassInput.vue";
import { Preferences } from "@capacitor/preferences";
import router from "@/router";

let pin = ref("");
let playShake = ref(false);
let playSuccess = ref(false);

const login = async () => {
  console.log(pin);
  let registeredPin = await Preferences.get({ key: "pin" });
  if (pin.value === registeredPin.value) {
    console.log("success");
    playSuccess.value = true;
    setTimeout(() => {
      router.push("/home");
      // playSuccess.value = false;
      // pin.value = "";
    }, 1500);
    // router.push("/home");
  } else {
    playShake.value = true;
    setTimeout(() => {
      playShake.value = false;
      pin.value = "";
    }, 500);
  }
};

const checkPass = () => {
  if (pin.value.length === 6) {
    login();
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
