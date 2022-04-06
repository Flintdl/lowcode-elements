<template>
  <div
    class="sf-bg-primary sf-position-relative sf-overflow-hidden"
    :style="cssDefinido"
    ref="box"
    @click="getData"
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

      {{ tamanho }}% <br />{{ tamanhoPixel }}px
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
      tamanhoPixel: null,
    };
  },
  props: {
    cssDefinido: Object,
    tamanho: String,
    altura: String,
  },
  methods: {
    getData: function () {
      console.log("ensad");
      this.tamanhoPixel = this.$refs.box.offsetWidth;
      console.log(this.tamanhoPixel);
    },
  },
};
</script>

<style></style>
