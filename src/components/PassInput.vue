<template>
  <div class="input" :class="playSuccess ? 'play-success' : ''">
    <div class="big-dot">
      <svg class="animated-check" viewBox="0 0 24 24">
        <path d="M4.1 12.7L9 17.6 20.3 6.3" fill="none" />
      </svg>
    </div>
    <div class="dots" :class="playShake ? 'play-shake' : ''">
      <div class="dot" :class="modelValue.length >= 1 ? 'visible' : ''"></div>
      <div class="dot" :class="modelValue.length >= 2 ? 'visible' : ''"></div>
      <div class="dot" :class="modelValue.length >= 3 ? 'visible' : ''"></div>
      <div class="dot" :class="modelValue.length >= 4 ? 'visible' : ''"></div>
      <div class="dot" :class="modelValue.length >= 5 ? 'visible' : ''"></div>
      <div class="dot" :class="modelValue.length >= 6 ? 'visible' : ''"></div>
    </div>
    <input
      type="text"
      :value="modelValue"
      @input="updateValue"
      :placeholder="placeholder"
      maxlength="6"
      spellcheck="false"
    />
  </div>
</template>

<script setup>
const props = defineProps({
  modelValue: String,
  placeholder: String,
  playShake: {
    type: Boolean,
    default: false,
  },
  playSuccess: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(["update:modelValue"]);

const updateValue = (event) => {
  emit("update:modelValue", event.target.value);
};
</script>

<style lang="scss" scoped>
.input {
  position: relative;
  height: 50px;
  min-width: 200px;
  // padding: 0 20px;
  margin: 10px 0;

  display: flex;
  justify-content: center;
  align-items: center;

  transition: 0.3s;

  .dots {
    width: 70%;
    height: 100%;

    position: absolute;
    // background: #ffffff55;
    z-index: 1;

    // enable click through
    pointer-events: none;

    display: flex;
    justify-content: space-evenly;
    align-items: center;
    transition: 0.3s;

    .dot {
      width: 0px;
      height: 0px;

      max-width: 150px;
      max-height: 150px;

      // flex: 0;
      background: #d7d1d1;
      border-radius: 50%;

      transition: 0.3s;
    }
    .visible {
      height: 10px;
      width: 10px;
    }
  }

  input {
    height: 100%;
    width: 100%;
    outline: none;
    border: none;

    border-radius: 10px;
    background-color: #302d2d;
    color: #302d2d;

    text-align: center;
    transition: 0.3s;
  }
}

.big-dot {
  position: absolute;

  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;

  width: 0;
  height: 0;
  border-radius: 50%;
  background: #4aff44;
  z-index: 1;

  transition: 0.3s;

  .animated-check {
    height: 0;
    width: 0;

    transition: 0.3s;
  }
}
// play-shake class
.play-shake {
  // shake dot in wave motion
  .dot.visible {
    animation: wobble 0.5s;
    animation-iteration-count: 1;
    background: #f55454 !important;
  }

  //   animation: shake 0.5s;
  animation-iteration-count: 1;
}

.play-success {
  height: 200px;
  width: 200px;
  input {
    // turn to circle
    border-radius: 50%;
  }
  //   arrange the dots in a circle
  .big-dot {
    width: 150px;
    height: 150px;

    transition: 0.3s 0.5s;

    display: flex;
    justify-content: center;
    align-items: center;

    .animated-check {
      height: 100px;
      width: 100px;
      path {
        fill: none;
        stroke: #f5faf2;
        stroke-width: 4;
        stroke-dasharray: 23;
        stroke-dashoffset: 23;
        animation: draw 0.3s 0.7s linear forwards;
        stroke-linecap: round;
        stroke-linejoin: round;
      }
    }
  }
  .dots {
    width: 0%;
    height: 0%;

    .dot {
      width: 0px;
      height: 0px;
      transition: 0.3s 0.1s;
    }
  }
}

@keyframes draw {
  to {
    stroke-dashoffset: 0;
  }
}

// wobble dots
@keyframes wobble {
  0% {
    transform: translate(1px, 1px) rotate(0deg);
  }
  10% {
    transform: translate(-1px, -2.5px) rotate(-1deg);
  }
  20% {
    transform: translate(-5px, 0px) rotate(1deg);
  }
  30% {
    transform: translate(5px, 2.5px) rotate(0deg);
  }
  40% {
    transform: translate(1px, -1px) rotate(1deg);
  }
  50% {
    transform: translate(-1px, 2.5px) rotate(-1deg);
  }
  60% {
    transform: translate(-5px, 1px) rotate(0deg);
  }
  70% {
    transform: translate(5px, 1px) rotate(-1deg);
  }
  80% {
    transform: translate(-1px, -1px) rotate(1deg);
  }
  90% {
    transform: translate(1px, 2.5px) rotate(0deg);
  }
  100% {
    transform: translate(1px, -2.5px) rotate(-1deg);
  }
}
</style>
