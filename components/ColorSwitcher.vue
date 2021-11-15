<template>
  <div>
    <select v-model="$colorMode.preference" class="form-select color-selector">
      <option value="system">Auto</option>
      <option value="light">☀️</option>
      <option value="dark">🌗</option>
    </select>
  </div>
</template>

<script>
export default {
  setup() {
    return {}
  },
}
</script>

<style lang="scss" scoped>
svg {
    fill: var(--color-primary);
}
.color-selector {
  position: fixed;
  width: auto;
  z-index: 9999;
  border-radius: 25px;
  bottom: 25px;
  right: 25px;
  height: 2rem;
  width: auto;
  border: none;
  background: var(--color);
  color: var(--bg)
}
</style>