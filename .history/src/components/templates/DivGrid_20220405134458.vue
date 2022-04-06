<template>
  <div
    class="xd sf-bg-primary sf-position-relative sf-overflow-hidden"
    :style="cssDefinido"
  >
    <span
      class="sf-position-absolute sf-centerAbsolute-xy sf-text-white sf-font-weight-extra-bold sf-text-center"
      :style="tamanho"
    >
      <i
        class="mdi mdi-arrow-up sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      <i
        class="mdi mdi-arrow-down sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      {{ altura }}px <br />

      {{ tamanho }}% {{ tamanhoPixel }}PX
      <i
        class="mdi mdi-arrow-left sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      <i
        class="mdi mdi-arrow-right sf-text-md sf-text-white sf-dropdown-action"
      ></i>
    </span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tamanhoPixel: document.querySelector(".xd").offsetWidth,
    };
  },
  props: {
    cssDefinido: Object,
    tamanho: String,
    altura: String,
  },
};
</script>

<style></style>
