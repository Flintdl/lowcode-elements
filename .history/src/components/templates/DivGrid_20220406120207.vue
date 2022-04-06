<template>
  <div
    class="sf-position-relative sf-overflow-hidden"
    :style="cssDefinido"
    ref="container"
  >
    <div
      class="sf-height-position sf-position-absolute sf-display-flex sf-flex-column sf-align-items-center sf-px-2"
    >
      <i
        class="mdi mdi-arrow-up sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      <span
        class="value sf-text-white sf-font-weight-extra-bold sf-text-center"
      >
        {{ altura }}
      </span>
      <span class="sf-text-white sf-font-weight-extra-bold sf-text-center">
        px
      </span>
      <i
        class="mdi mdi-arrow-down sf-text-md sf-text-white sf-dropdown-action"
      ></i>
    </div>
    <div class="sf-width-position sf-position-absolute">
      <span class="sf-text-white sf-font-weight-extra-bold sf-text-center">
        {{ larguraPixel }}px
      </span>
      <i
        class="mdi mdi-arrow-left sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      <i
        class="mdi mdi-arrow-right sf-text-md sf-text-white sf-dropdown-action"
      ></i>
    </div>
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

<style lang="scss" scoped>
.sf-height-position {
  transform: translateY(-50%);
  left: 0;
  top: 50%;
  span.value {
    word-break: break-all;
    line-height: 1;
    width: 10px;
  }
}
</style>
