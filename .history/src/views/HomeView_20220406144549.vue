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
                v-for="linha of Number(quantidadeLinha)"
              >
                <div :style="widthProps" class="sf-position-absolute"></div>
              </span>
            </div>
          </div>
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
              :cssDefinido="cssProps"
              :cssDivPai="cssDivPai"
              class="sf-position-relative"
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
      gridLinhas: null,
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
    quantidadeLinha: String,
    inputBorderRadius: String,
    checkValue: String,
    inputColor: String,
    inputBorder: String,
    inputBorderColor: String,
    inputBorderTipo: String,
  },
  computed: {
    cssDivPai() {
      return {
        width: `calc(${this.valor}% - ${this.inputGap}px)`,
      };
    },
    cssProps() {
      return {
        flex: `${this.quantidade} 0 ${this.valor}%`,
        height: this.valorHeight + "px",
        "border-radius": this.inputBorderRadius + "px",
        "background-color": this.inputColor,
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
        gap: this.inputGap + "px",
      };
    },
    widthProps1() {
      return {
        width: `calc((${this.valor}% - ${this.inputGap}px * ${
          this.quantidadeLinha - "1"
        }) / ${this.quantidadeLinha})`,
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
    this.arroz = this.$refs.previewGrid?.offsetHeight;
  },
  created() {
    this.$watch(["quantidade", "quantidadeLinhas"], () => {
      this.arroz = this.$refs.previewGrid?.offsetHeight;
    });
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
