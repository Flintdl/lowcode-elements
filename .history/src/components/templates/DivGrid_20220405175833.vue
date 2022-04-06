<template>
  <div
    class="sf-position-relative sf-overflow-hidden"
    :style="cssDefinido"
    :onresize="screenWidth"
    ref="container"
  >
    {{ arroz }}
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

      {{ tamanho }}%
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
      arroz: null,
    };
  },
  mounted() {
    this.screenWidth();
  },
  methods: {
    screenWidth() {
      this.arroz = this.$refs.container?.offsetWidth;
      console.log(this.$refs.container.offsetWidth);
    },
  },
  props: {
    cssDefinido: Object,
    tamanho: String,
    altura: String,
  },
  created() {
    this.$watch("tamanho", () => {
      this.screenWidth;
    });
  },
};
</script>

<style></style>
