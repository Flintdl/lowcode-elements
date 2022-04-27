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
            ></div>
          </div>
          <TransitionGroup
            name="bounce-grid"
            :style="gapProps"
            :class="[
              'sf-display-grid sf-flex-wrap sf-width-100',
              'sf-justify-content-' + checkValue,
            ]"
          >
            <DivGrid
              v-bind:key="n"
              :ref="'bloco_' + n"
              :idReferencia="n"
              v-for="n of quantidade"
              :altura="String(valorHeight)"
              :idBloco="'container_' + n"
              :idGrid="'container_' + n"
              :cssDefinido="cssProps"
              class="sf-position-relative"
            />
          </TransitionGroup>
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
// import Prism from "prismjs";
// import "prismjs/themes/prism-okaidia.min.css";

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
    inputGap: String,
    quantidade: Number,
    valorHeight: String,
    quantidadeLinha: Number,
    checkValue: String,
    inputGrid: String,
  },
  computed: {
    cssProps() {
      return {
        // width: `calc((${this.valor}% - ${this.inputGap}px * ${
        //   this.quantidadeLinha - "1"
        // }) / ${this.quantidadeLinha})`,
        "min-height": this.valorHeight + "px",
        height: "fit-content",
      };
    },
    gapProps() {
      return {
        "grid-template-columns": `repeat(${this.quantidadeLinha}, calc(100% / ${this.quantidadeLinha}))`,
        gap: this.inputGap + "px",
      };
    },
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
.bounce-grid-enter-active {
  animation: bounce-in-grid 0.3s;
}
.bounce-grid-leave-active {
  animation: bounce-in-grid 0.15s reverse;
}
@keyframes bounce-in-grid {
  0% {
    transform: scale(0.5);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}
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
