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
            :tamanho="String(valor)"
            class="sf-position-relative"
            :width="cssProps"
          />
        </div>
      </div>
    </div>
    <div class="sf-row">
      <div class="sf-col-12">
        <div
          class="sf-col-8 sf-mx-auto sf-mt-5 sf-code-content sf-p-4 sf-border-radius sf-bg-code"
        >
          <code style="">
            <pre>{{ htmlPreviewCode }}</pre>
          </code>
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
  data: function () {
    return {
      htmlPreviewCode: document.getElementById("sf-preview-code").innerHTML,
    };
  },
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
  background-color: #ff008c0a;
  border: 1px solid rgba(255, 0, 140, 0.103);
  border-left: 2px solid #ff008c38;
  border-right: 2px solid #ff008c38;
  position: absolute;
}
</style>
