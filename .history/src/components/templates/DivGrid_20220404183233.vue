<template>
  <div
    class="sf-bg-secondary sf-position-relative sf-border-radius"
    :style="width"
  >
    <span
      class="sf-position-absolute sf-centerAbsolute-xy sf-text-white sf-font-weight-extra-bold"
      :style="tamanho"
    >
      {{ tamanho }}%
    </span>
  </div>
</template>

<script>
export default {
  props: {
    width: String,
    tamanho: String,
  },
};
</script>

<style></style>
