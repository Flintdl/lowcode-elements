<template>
  <div class="sf-row">
    <InputBackgroundColorBloco
      :valorInput="blocoProps.backgroundColor"
      v-on:callback="getValorBackgroundColor"
    />
    <div class="sf-col-12 sf-form-group">
      <span
        class="sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
      >
        Dimensões
        <hr class="sf-width-100 sf-ml-4" />
      </span>
    </div>
    <InputLarguraBloco
      :valorInput="blocoProps.width"
      v-on:callback="getValorWidth"
    />
    <InputAlturaBloco
      :valorInput="blocoProps.height"
      v-on:callback="getValorHeight"
    />
    <InputMinimoAlturaBloco
      :valorInput="blocoProps.minheight"
      v-on:callback="getValorMinHeight"
    />
    <InputMaximoAlturaBloco
      :valorInput="blocoProps.maxheight"
      v-on:callback="getValorMaxHeight"
    />
    <div class="sf-col-12 sf-form-group">
      <span
        class="sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
      >
        Bordas
        <hr class="sf-width-100 sf-ml-4" />
      </span>
    </div>
    <InputBorderRadiusBloco
      :valorInput="blocoProps.bordas.borderRadius"
      v-on:callback="getValorRadius"
    />
    <InputBorderBloco
      :valorInput="blocoProps.bordas.borderWidth"
      v-on:callback="getValorBorder"
    />
    <InputBorderTipoBloco
      :valorInput="blocoProps.bordas.borderTipo"
      v-on:callback="getValorBorderTipo"
    />
    <InputBorderCorBloco
      :valorInput="blocoProps.bordas.borderColor"
      v-on:callback="getValorBorderColor"
    />
    <div class="sf-col-12 sf-form-group">
      <span
        class="sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
      >
        Posição
        <hr class="sf-width-100 sf-ml-4" />
      </span>
    </div>
    <SelectPosicaoBloco v-on:callback="getValorPosicao" />
  </div>
</template>

<script>
import InputLarguraBloco from "@/components/templates/InputLarguraBloco.vue";
import InputAlturaBloco from "@/components/templates/InputAlturaBloco.vue";
import SelectPosicaoBloco from "@/components/templates/Dialog/InputsDialog/SelectPosicaoBloco.vue";
import InputMinimoAlturaBloco from "@/components/templates/Dialog/InputsDialog/InputMinimoAlturaBloco.vue";
import InputMaximoAlturaBloco from "@/components/templates/Dialog/InputsDialog/InputMaximoAlturaBloco.vue";
import InputBackgroundColorBloco from "@/components/templates/Dialog/InputsDialog/InputBackgroundColorBloco.vue";

// Bordas
import InputBorderRadiusBloco from "@/components/templates/InputBorderRadiusBloco.vue";
import InputBorderBloco from "@/components/templates/InputBorderBloco.vue";
import InputBorderCorBloco from "@/components/templates/InputBorderCorBloco.vue";
import InputBorderTipoBloco from "@/components/templates/InputBorderTipoBloco.vue";

export default {
  data: function () {
    return {
      blocoProps: {
        id: this.idBloco,
        width: "100",
        height: "500",
        minheight: "150",
        maxheight: "250",
        backgroundColor: "#FFFFFF",
        posicao: "0 auto",
        bordas: {
          borderRadius: "4",
          borderWidth: "0",
          borderTipo: "initial",
          borderColor: "#ffffff00",
        },
      },
    };
  },
  components: {
    InputLarguraBloco,
    InputAlturaBloco,
    SelectPosicaoBloco,
    InputMinimoAlturaBloco,
    InputMaximoAlturaBloco,
    InputBackgroundColorBloco,
    // Bordas
    InputBorderRadiusBloco,
    InputBorderBloco,
    InputBorderCorBloco,
    InputBorderTipoBloco,
  },
  props: {
    idBloco: String,
  },
  methods: {
    getValorWidth(valor) {
      this.blocoProps.width = valor;
      this.onSelectType();
    },
    getValorHeight(valor) {
      this.blocoProps.height = valor;
      this.onSelectType();
    },
    getValorMinHeight(valor) {
      this.blocoProps.minheight = valor;
      this.onSelectType();
    },
    getValorMaxHeight(valor) {
      this.blocoProps.maxheight = valor;
      this.onSelectType();
    },
    getValorBackgroundColor(valor) {
      this.blocoProps.backgroundColor = valor;
      this.onSelectType();
    },
    getValorRadius(valor) {
      this.blocoProps.bordas.borderRadius = valor;
      this.onSelectType();
    },
    getValorBorder(valor) {
      this.blocoProps.bordas.borderWidth = valor;
      this.onSelectType();
    },
    getValorBorderColor(valor) {
      this.blocoProps.bordas.borderColor = valor;
      this.onSelectType();
    },
    getValorBorderTipo(valor) {
      this.blocoProps.bordas.borderTipo = valor;
      this.onSelectType();
    },
    getValorPosicao(valor) {
      this.blocoProps.posicao = valor;
      this.onSelectType();
    },
    onSelectType() {
      this.$emit("callbackPropsBloco", this.blocoProps);
    },
    porcentagemRange(e, tipoFuncao) {
      var conversorQuantidade;
      switch (tipoFuncao) {
        case "width":
          conversorQuantidade = "";
          break;
        case "height":
          conversorQuantidade = "/ 5";
          break;
        default:
          conversorQuantidade = "* 2";
          break;
      }
      var h4Capture = e.currentTarget.nextElementSibling;
      h4Capture = h4Capture.querySelector("h4");
      h4Capture.innerHTML = e.currentTarget.value + "<span></span>";
      e.currentTarget.style.filter =
        "hue-rotate(" + e.currentTarget.value + "deg)";
      h4Capture.style.cssText = `transform: translateX(calc(-50% - 20px))
      }); left: calc(${e.currentTarget.value}% ${conversorQuantidade})`;
      h4Capture.style.cssText = `transform: translateX(-50%)
      }); left: calc(${e.currentTarget.value}% ${conversorQuantidade})`;
      h4Capture.style.filter = "hue-rotate(" + e.currentTarget.value + "deg)";
    },
  },
};
</script>
