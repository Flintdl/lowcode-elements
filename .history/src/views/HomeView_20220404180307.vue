<template>
  <div>
    {{ checkValue }}
    <div class="sf-row">
      <div id="sf-preview-code-content" class="sf-col-12 sf-mx-auto">
        <div
          id="sf-preview-code"
          :style="gapProps"
          class="sf-display-flex sf-flex-wrap sf-display-flex sf-width-100"
        >
          <DivGrid
            v-bind:key="n"
            v-for="n of quantidade"
            :tamanho="valor"
            class="sf-position-relative"
            :width="cssProps"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import DivGrid from "@/components/templates/DivGrid.vue";

export default {
  name: "HomeView",
  components: {
    DivGrid,
  },
  props: {
    valor: Number,
    inputGap: Number,
    quantidade: Number,
    valorHeight: Number,
    quantidadeLinha: Number,
    checkValue: String,
  },
  computed: {
    cssProps() {
      return {
        width: `calc(${this.valor / this.quantidadeLinha}% - ${
          this.inputGap
        }px)`,
        height: this.valorHeight + "px",
      };
    },
    gapProps() {
      return {
        gap: this.inputGap + "px",
        "justify-content": this.checkValue,
      };
    },
  },
};
</script>

<style>
#sf-preview-code div:after {
  content: "";
  width: 100%;
  height: 150%;
  top: -25%;
  background-color: rgba(255, 0, 140, 0.082);
  border: 1px solid rgba(255, 0, 140, 0.103);
  border-left: 2px solid rgba(255, 0, 140, 0.589);
  border-right: 2px solid rgb(255, 0, 140, 0.589);
  position: absolute;
}
</style>
