<template>
  <div
    class="sf-position-relative sf-overflow-hidden"
    :style="cssDefinido"
    ref="container"
  >
    <
    <i class="mdi mdi-arrow-up sf-text-md sf-text-white sf-dropdown-action"></i>
    <span> {{ altura }}px </span>
    <i
      class="mdi mdi-arrow-down sf-text-md sf-text-white sf-dropdown-action"
    ></i>

    {{ larguraPixel }}px
    <i
      class="mdi mdi-arrow-left sf-text-md sf-text-white sf-dropdown-action"
    ></i>
    <i
      class="mdi mdi-arrow-right sf-text-md sf-text-white sf-dropdown-action"
    ></i>
    <span
      class="sf-position-absolute sf-centerAbsolute-xy sf-text-white sf-font-weight-extra-bold sf-text-center"
      :style="tamanho"
    >
    </span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      larguraPixel: null,
    };
  },
  mounted() {
    this.larguraPixel = this.$refs.container?.offsetWidth;
  },

  props: {
    cssDefinido: Object,
    tamanho: String,
    altura: String,
  },
  created() {
    this.$watch("tamanho", () => {
      this.larguraPixel = this.$refs.container?.offsetWidth;
    });
  },
};
</script>

<style></style>
