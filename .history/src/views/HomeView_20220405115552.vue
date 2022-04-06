<template>
  <div>
    <div class="sf-row">
      <div
        id="sf-preview-code-content"
        class="sf-col-12 sf-mx-auto sf-position-relative"
      >
        <!-- <div class="sf-display-flex sf-flex-wrap sf-width-100">
          <div
            id="sf-grid-template"
            class="sf-grid sf-position-relative"
            :style="{
              width: cssProps.width,
              height: String(valorHeight) + 'px',
              left: inputGap / 2 + 'px',
            }"
            v-bind:key="n"
            v-for="n of quantidade"
          ></div>
        </div> -->
        <div id="sf-preview-code">
          <div
            :style="gapProps"
            :class="[
              'sf-display-flex sf-flex-wrap sf-width-100',
              'sf-justify-content-' + checkValue,
            ]"
          >
            <DivGrid
              v-bind:key="n"
              v-for="n of quantidade"
              :tamanho="String(valor)"
              :altura="String(valorHeight)"
              class="sf-position-relative"
              :cssDefinido="cssProps"
            />
          </div>
        </div>
      </div>
      <div class="sf-col-12 sf-mt-3 sf-display-flex sf-justify-content-end">
        <button class="sf-btn sf-btn-primary" @click="gerarCodigo">
          Gerar Codigo
        </button>
      </div>
    </div>
    <div class="sf-row" v-if="htmlPreviewCode">
      <div class="sf-col-12 sf-mx-auto sf-mt-5 sf-code-content">
        <div class="sf-bg-code sf-p-4 sf-border-radius">
          <code>
            <span class="sf-text-info">Código do Preview</span> <br />
            {{ htmlPreviewCode }}
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
      htmlPreviewCode: null,
    };
  },
  components: {
    DivGrid,
  },
  props: {
    valor: String,
    inputGap: String,
    quantidade: Number,
    valorHeight: String,
    quantidadeLinha: String,
    inputBorderRadius: String,
    checkValue: String,
  },
  computed: {
    cssProps() {
      return {
        width: `calc((${this.valor}% - ${this.inputGap}px * ${
          this.quantidadeLinha - "1"
        }) / ${this.quantidadeLinha})`,
        height: this.valorHeight + "px",
        "border-radius": this.inputBorderRadius + "px",
      };
    },
    gapProps() {
      return {
        gap: this.inputGap + "px",
      };
    },
  },
  methods: {
    gerarCodigo() {
      this.htmlPreviewCode =
        document.getElementById("sf-preview-code").innerHTML;
    },
  },
};
</script>

<style>
#sf-grid-template:after {
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
