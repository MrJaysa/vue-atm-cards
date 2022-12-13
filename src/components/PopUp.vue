<template>
  <div class="popup">
    <div class="popup-inner">
      <slot />
      <button class="popup-close" @click="TogglePopup()">Continue</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["TogglePopup"],
};
</script>

<style scoped>
/* CSS Variables */
:root {
  --white: hsl(0, 0%, 100%);
  --LightGrayishViolet: hsl(270, 3%, 87%);
  --DarkGrayishViolet: hsl(279, 6%, 55%);
  --VeryDarkViolet: hsl(278, 68%, 11%);
  --shadow: 5px 5px 5px rgba(104, 104, 104, 0.2);
  --font: "Space Grotesk", sans-serif;
}

.popup {
  position: fixed;
  z-index: 99;
  background: var(--white);
  display: flex;
  align-items: center;
  justify-content: center;
  width: 71.5%;
  height: 46.5rem;
}
.popup-inner {
  padding: 32px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.popup-close {
  background: var(--VeryDarkViolet);
  width: 20rem;
  margin-top: 2rem;
}
/* media queries */
@media (max-width: 500px) {
  .popup {
    margin-top: -11.2rem;
    margin-left: 7rem;
    width: 100%;
  }
}
</style>
