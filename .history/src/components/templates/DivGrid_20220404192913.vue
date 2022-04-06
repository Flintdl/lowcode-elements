<template>
  <div
    class="sf-bg-primary sf-position-relative sf-border-radius"
    :style="width"
  >
    <span
      class="sf-position-absolute sf-centerAbsolute-xy sf-text-white sf-font-weight-extra-bold"
      :style="tamanho"
    >
      {{ altura }}px {{ tamanho }}%
    </span>
  </div>
</template>

<script>
export default {
  props: {
    width: String,
    tamanho: String,
    altura: String,
  },
};
</script>

<style></style>
