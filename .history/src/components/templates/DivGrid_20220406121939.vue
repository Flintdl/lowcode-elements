<template>
  <div
    class="sf-position-relative sf-overflow-hidden"
    :style="cssDefinido"
    ref="container"
  >
    {{ linhas }}
    <div
      class="sf-height-position sf-position-absolute sf-display-flex sf-flex-column sf-px-2"
    >
      <i
        class="mdi mdi-arrow-up sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      <span class="sf-text-white sf-font-weight-extra-bold sf-text-center">
        {{ altura }}
        px
      </span>
      <i
        class="mdi mdi-arrow-down sf-text-md sf-text-white sf-dropdown-action"
      ></i>
    </div>
    <div
      class="sf-width-position sf-width-100 sf-position-absolute sf-display-flex sf-align-items-center sf-px-2"
    >
      <i
        class="mdi mdi-arrow-left sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      <hr class="sf-width-100 sf-mr-2" />
      <span class="sf-text-white sf-font-weight-extra-bold sf-text-center">
        {{ larguraPixel }}px
      </span>
      <hr class="sf-width-100 sf-ml-2" />
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
    linhas: Number,
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
.sf-height-position,
.sf-width-position {
  font-size: 1.4ch;
}
.sf-height-position {
  transform: translateY(-50%);
  left: 0;
  top: 50%;
}
.sf-width-position {
  transform: translateX(-50%);
  bottom: 0;
  left: 50%;
}
</style>
