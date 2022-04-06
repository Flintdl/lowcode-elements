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
        class="mdi mdi-arrow-left sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      <i
        class="mdi mdi-arrow-right sf-text-md sf-text-white sf-dropdown-action"
      ></i
      ><br />
      {{ altura }}px <br />
      <i
        class="mdi mdi-arrow-left sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      <i
        class="mdi mdi-arrow-right sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      {{ tamanho }}%
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
