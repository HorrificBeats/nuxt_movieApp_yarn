<template>
  <!--  the `app` Css Class applies props to all pages   -->
  <div class="app">
    <select v-model="$colorMode.preference" class="form-select color-selector">
      <option value="system">Auto</option>
      <option value="light">☀️</option>
      <option value="dark">🌗</option>
    </select>

    <Nuxt keep-alive />
  </div>
</template>

<style lang="scss" scoped>
.color-selector {
  position: fixed;
  width: auto;
  z-index: 9999;
  border-radius: 25px;
  bottom: 25px;
  right: 25px;
}
</style>
