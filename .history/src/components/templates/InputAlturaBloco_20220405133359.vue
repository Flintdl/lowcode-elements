<template>
  <div class="sf-col-6 sf-form-group">
    <label class="sf-label">Altura Bloco</label>
    <div class="sf-input-group">
      <div
        class="sf-input-group-append sf-display-flex sf-justify-content-center"
      >
        <div
          class="sf-display-flex sf-align-items-center sf-justify-content-center sf-border-left-0 sf-border-radius sf-border-radius-left-0 sf-pr-2 sf-text-info sf-font-weight-bold sf-min-80-px"
        >
          <div class="sf-input-group">
            <input
              type="number"
              value="1"
              min="1"
              max="500"
              v-model="valorHeight"
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
      <input
        type="range"
        min="10"
        max="500"
        id="valorHeight"
        v-model="valorHeight"
        class="sf-px-0 sf-border-right-0 sf-border-radius-right-0 sf-text-capitalize"
      />
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
};
</script>

<style></style>
