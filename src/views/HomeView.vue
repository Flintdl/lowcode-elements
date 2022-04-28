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
              :id="'container_' + n"
              v-for="n of quantidade"
              :altura="String(valorHeight)"
              :idBloco="'container_' + n"
              :idGrid="'container_' + n"
              :cssDefinido="cssProps"
              class="sf-position-relative"
            />
          </TransitionGroup>
        </div>

        <div class="sf-container-preview">
          <pre v-if="htmlPreviewCode.length" class="language-markup">
  <code id="preview-code-html" class="language-markup" style="user-select: text!important">
    <span v-for="(hpc, i) of htmlPreviewCode" v-bind:key="i">
    {{hpc.tagInicial}}
    <span v-for="(tag, j) of hpc.tagTag" v-bind:key="j" style="line-height: 1 !important">
      {{tag.tag}}
    </span>
    {{hpc.tagFinal}}
    </span>
  </code>
</pre>
          <pre v-if="cssPropsGeneration.length" class="language-css">
  <code id="preview-code" class="language-css" style="user-select: text!important">
      <span v-for="(styleprops, i) of cssPropsGeneration" v-bind:key="'a'+i" style="user-select: text!important">
    #{{styleprops.id}} {
      width: {{styleprops.style.width}}px;
      overflow: auto;
      padding: 1.5rem;
      box-shadow: {{styleprops.style.boxShadow}};
      min-height: {{styleprops.style.minHeight}};
      height: {{styleprops.style.height}};
      max-height: {{styleprops.style.maxHeight}};
      background-color: {{styleprops.style.backgroundColor}};
      margin: {{styleprops.style.margin}};
      border-radius: {{styleprops.style.borderRadius}};
    }
      </span>
  </code>
</pre>
        </div>
      </div>
      <div class="sf-col-12 sf-mt-3 sf-display-flex sf-justify-content-end">
        <button class="sf-btn sf-btn-primary" @click="gerarCodigo">
          Gerar Codigo
        </button>
      </div>
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
      htmlPreviewCode: [],
      gridLinhas: null,
      valorRecalculado: null,
      arroz: null,
      widthGeneration: null,
      cssPropsGeneration: [],
    };
  },
  components: {
    DivGrid,
  },
  mounted() {
    window.Prism = window.Prism || {};
    window.Prism.manual = true;
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
      // var containerGRID = document.querySelector(`.sf-display-grid`).innerHTML;

      if (localStorage.getItem("textLabelBloco")) {
        var listaGet = JSON.parse(localStorage.getItem("textLabelBloco")) || [];
        listaGet.forEach((element, index) => {
          var containerID = document.querySelector(`#${element.container_id}`);
          if (containerID) {
            var containerComputed = getComputedStyle(containerID);
            var el = this.htmlPreviewCode[index] || {
              tagInicial: "",
              tagTag: [],
              tagFinal: "",
            };

            el.tagInicial = `<div id="${element.container_id}"><div class="sf-row">`;
            element.props.forEach((element) => {
              console.log(this.htmlPreviewCode);
              el.tagTag.push({
                tag:
                  element &&
                  `<div class='sf-col-12'><${element.tag}>${element.texto}</${element.tag}></div>`,
                // "<div class='sf-col-12'>" +
                //   "<" +
                //   element.tag +
                //   ">" +
                //   element.texto +
                //   "</" +
                //   element.tag +
                //   ">" +
                //   "</div>",
              });
            });
            el.tagFinal = `</div></div>`;

            this.htmlPreviewCode[index] = el;

            console.log(JSON.stringify(this.htmlPreviewCode));

            var boxShadow = containerComputed.getPropertyValue("box-shadow");
            var minHeight = containerComputed.getPropertyValue("min-height");
            var height = containerComputed.getPropertyValue("height");
            var maxHeight = containerComputed.getPropertyValue("max-height");
            var backgroundColor =
              containerComputed.getPropertyValue("background-color");
            var margin = containerComputed.getPropertyValue("margin");
            var borderRadius =
              containerComputed.getPropertyValue("border-radius");
            var width = containerID.offsetWidth;

            this.cssPropsGeneration.push({
              id: element.container_id,
              style: {
                width: width,
                boxShadow: boxShadow,
                minHeight: minHeight,
                height: height,
                maxHeight: maxHeight,
                backgroundColor: backgroundColor,
                margin: margin,
                borderRadius: borderRadius,
              },
            });
          }
        });
      }

      setTimeout(() => {
        Prism.highlightAll();
      }, 50);
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
code[class*="language-markup"],
pre[class*="language-markup"] {
  line-height: 0.9;
}
</style>
