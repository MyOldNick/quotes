<template>
  <div class="header"><p class="header-logo">CRYPTO</p></div>
</template>
<script>
export default {
  name: "Header",
};
</script>
<style scoped>
.header {
  width: 100%;
  height: 60px;
  background-color: #191a20;
  text-align: center;
}

.header-logo {
  font-size: 38px;
  background: linear-gradient(
    90deg,
    rgba(31, 70, 106, 1) 14%,
    rgba(255, 255, 255, 0) 50%,
    rgba(255, 255, 255)
  );
  background-clip: text;
  -webkit-text-fill-color: transparent;
  user-select: none;
}
</style>