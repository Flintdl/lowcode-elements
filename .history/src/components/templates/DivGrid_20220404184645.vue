<template>
  <div
    class="sf-bg-primary sf-position-relative sf-border-radius"
    :style="width && height"
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
    height: String,
    tamanho: String,
  },
};
</script>

<style></style>
