<template>
  <div
    class="sf-col-6 sf-form-group sf-justify-content-between sf-display-flex sf-flex-wrap"
  >
    <label class="sf-label">Largura</label>
    <span class="sf-font-weight-bold sf-text-muted"> {{ valorInput }} % </span>
    <input
      type="range"
      min="0"
      max="100"
      id="inputRange"
      @input="porcentagemRange($event, 'width')"
      v-model="valor"
      class="sf-px-0 sf-text-capitalize input-range-animation"
    />
    <div class="h4-container">
      <div class="h4-subcontainer">
        <h4>0<span></span></h4>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    valorInput: String,
  },
  computed: {
    valor: {
      get() {
        var inp = this.valorInput;
        return inp;
      },
      set(valorInput) {
        this.$emit("callback", valorInput);
      },
    },
  },
  methods: {
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

<style></style>
