<template>
  <div>
    <div class="sf-row">
      <div
        id="sf-preview-code-content"
        class="sf-col-12 sf-mx-auto sf-position-relative"
      >
        <div id="sf-preview-code" ref="previewGrid">
          <div class="sf-row">
            <div
              :class="[
                'sf-col-12 sf-text-center sf-display-flex sf-flex-wrap',
                'sf-justify-content-' + checkValue,
              ]"
            >
              <span
                v-bind:key="linha"
                :style="widthProps1"
                class="sf-display-block sf-position-relative"
                v-for="linha of 8"
              >
                <div :style="widthProps" class="sf-position-absolute"></div>
              </span>
            </div>
          </div>
          <div
            :style="gapProps"
            :class="[
              'sf-display-grid sf-flex-wrap sf-width-100',
              'sf-justify-content-' + checkValue,
            ]"
          >
            <DivGrid
              v-bind:key="n"
              :ref="'bloco_' + n"
              v-for="n of quantidade"
              :tamanho="String(valor)"
              :altura="String(valorHeight)"
              :idBloco="'container_' + n"
              :background="inputColor"
              :cssDefinido="cssProps"
              class="sf-position-relative"
            />
          </div>
        </div>

        <!-- <pre class="language-markup">
        <code id="preview-code" class="language-markup">
         <div>aaaa</div>
        </code>
      </pre> -->
      </div>
      <!-- <div class="sf-col-12 sf-mt-3 sf-display-flex sf-justify-content-end">
        <button class="sf-btn sf-btn-primary" @click="gerarCodigo">
          Gerar Codigo
        </button>
      </div> -->
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import DivGrid from "@/components/templates/DivGrid.vue";
import Prism from "prismjs";
import "prismjs/themes/prism-okaidia.min.css";

export default {
  name: "HomeView",
  data: function () {
    return {
      htmlPreviewCode: null,
      gridLinhas: null,
      valorRecalculado: null,
      arroz: null,
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
    quantidadeLinha: Number,
    inputBorderRadius: String,
    checkValue: String,
    inputColor: String,
    inputBorder: String,
    inputBorderColor: String,
    inputBorderTipo: String,
    inputGrid: String,
  },
  computed: {
    cssProps() {
      return {
        // width: `calc((${this.valor}% - ${this.inputGap}px * ${
        //   this.quantidadeLinha - "1"
        // }) / ${this.quantidadeLinha})`,
        height: this.valorHeight + "px",
        "border-radius": this.inputBorderRadius + "px",
        border:
          this.inputBorder +
          "px" +
          " " +
          this.inputBorderTipo +
          " " +
          this.inputBorderColor,
      };
    },
    gapProps() {
      return {
        "grid-template-columns": `repeat(${this.quantidadeLinha}, ${this.valorRecalculado}%)`,
        gap: this.inputGap + "px",
      };
    },
    widthProps1() {
      return {
        width: `10%`,
        visibility: this.inputGrid,
      };
    },
    widthProps() {
      return {
        height: this.arroz + "px",
        width: "clamp(50px, 50%, 100px)",
        "background-color": "#f7c7cfd1",
        border: "1px solid #f9edefd1",
        transform: "translateX(-50%)",
        "z-index": "500",
        left: "50%",
        top: "1px",
      };
    },
  },
  mounted() {
    window.Prism = window.Prism || {};
    window.Prism.manual = true;
    Prism.highlightAll();
    this.arroz = this.$refs.previewGrid?.clientHeight;
    this.valorRecalculado = this.valor / this.quantidadeLinha;
  },
  created() {
    this.$watch("quantidade", () => {
      this.arroz = this.$refs.previewGrid?.clientHeight;
      this.valorRecalculado = this.valor / this.quantidadeLinha;
    });
    this.$watch("quantidadeLinha", () => {
      this.arroz = this.$refs.previewGrid?.clientHeight;
      this.valorRecalculado = this.valor / this.quantidadeLinha;
    });
    this.$watch("valor", () => {
      this.arroz = this.$refs.previewGrid?.clientHeight;
      this.valorRecalculado = this.valor / this.quantidadeLinha;
    });
  },
  methods: {
    gerarCodigo() {
      this.htmlPreviewCode = true;
      document.getElementById("preview-code").innerHTML =
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
