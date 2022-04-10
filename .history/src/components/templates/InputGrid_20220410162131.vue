<template>
  <div class="sf-row">
    <div class="sf-col-12 sf-form-group">
      <span
        class="sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
      >
        Grids
        <hr class="sf-width-100 sf-ml-4" />
      </span>
    </div>
    <div class="sf-col-auto sf-form-group">
      <input
        id="visible"
        type="radio"
        name="positionDivs"
        v-model="valor"
        value="block"
        checked
      />
      <label for="visible"> Mostrar</label>
    </div>

    <div class="sf-col-auto sf-form-group">
      <input
        type="radio"
        id="hidden"
        name="positionDivs"
        v-model="valor"
        value="none"
      />
      <label for="hidden"> Esconder</label>
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
