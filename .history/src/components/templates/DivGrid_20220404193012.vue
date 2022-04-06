<template>
  <div
    class="sf-bg-primary sf-position-relative sf-border-radius"
    :style="width"
  >
    <span
      class="sf-position-absolute sf-centerAbsolute-xy sf-text-white sf-font-weight-extra-bold sf-text-center"
      :style="tamanho"
    >
      <i
        class="mdi mdi-arrow-left sf-text-xlg sf-text-muted sf-dropdown-action"
        ></i>
        class="mdi mdi-arrow-right sf-text-xlg sf-text-muted sf-dropdown-action"
      ></i>
      {{ altura }}px <br />{{ tamanho }}%
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
