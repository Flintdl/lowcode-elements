<template>
  <div
    class="sf-col-12 sf-form-group sf-justify-content-between sf-display-flex sf-flex-wrap"
  >
    <label class="sf-label">Shadow Bottom</label>
    <span class="sf-font-weight-bold sf-text-muted">
      {{ valorInput }} Pixels
    </span>

    <div
      class="sf-display-flex sf-flex-wrap sf-width-100 sf-align-items-center"
    >
      <input
        type="range"
        min="-50"
        max="50"
        @input="porcentagemRange"
        id="inputShadowBottom"
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
      }); left: calc(${e.currentTarget.value}% + 50%)`;
      h4Capture.style.cssText = `transform: translateX(-50%)
      }); left: calc(${e.currentTarget.value}% + 50%)`;
      h4Capture.style.filter = "hue-rotate(" + e.currentTarget.value + "deg)";
    },
  },
};
</script>

<style></style>
