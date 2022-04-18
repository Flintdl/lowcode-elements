<template>
  <div class="sf-col-12 sf-form-group">
    <label class="sf-label">Shadow do Bloco</label>
    <div class="sf-input-group sf-align-items-center">
      <div
        class="sf-input-group-append sf-display-flex sf-justify-content-center"
      >
        <div
          class="sf-display-flex sf-align-items-center sf-justify-content-center sf-border-left-0 sf-border-radius sf-border-radius-left-0 sf-pr-2 sf-text-info sf-font-weight-bold sf-min-80-px"
        >
          <div class="sf-input-group">
            <input
              type="number"
              min="0"
              max="50"
              v-model="valor"
              class="sf-text-md sf-height-fit-content sf-pr-0 sf-pl-1 sf-border-right-0 sf-border-radius-right-0 sf-text-capitalize"
              placeholder="Curva da Borda"
            />
            <div
              class="sf-input-group-append sf-display-flex sf-justify-content-center"
            >
              <div
                class="sf-display-flex sf-align-items-center sf-border sf-justify-content-center sf-border-left-0 sf-border-radius sf-border-radius-left-0 sf-pr-1 sf-text-info sf-font-weight-bold sf-text-sm"
              >
                PX
              </div>
            </div>
          </div>
        </div>
      </div>
      <div
        class="sf-display-flex sf-flex-wrap sf-width-100 sf-align-items-center"
      >
        <input
          type="range"
          min="0"
          max="50"
          @input="porcentagemRange"
          id="inputShadowExpanse"
          v-model="valor"
          class="sf-px-0 sf-text-capitalize input-range-animation"
        />
        <div class="h4-container">
          <div class="h4-subcontainer">
            <h4>0<span></span></h4>
          </div>
        </div>
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
        return this.valorInput;
      },
      set(valorInput) {
        this.$emit("callback", valorInput);
      },
    },
  },
  methods: {
    porcentagemRange(e) {
      var h4Capture = e.currentTarget.nextElementSibling;
      h4Capture = h4Capture.querySelector("h4");
      h4Capture.innerHTML = e.currentTarget.value + "<span></span>";
      e.currentTarget.style.filter =
        "hue-rotate(" + e.currentTarget.value + "deg)";
      h4Capture.style.cssText = `transform: translateX(calc(-50% - 20px))
      }); left: calc(${e.currentTarget.value}% * 2)`;
      h4Capture.style.cssText = `transform: translateX(-50%)
      }); left: calc(${e.currentTarget.value}% * 2)`;
      h4Capture.style.filter = "hue-rotate(" + e.currentTarget.value + "deg)";
    },
  },
};
</script>

<style></style>
